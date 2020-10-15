---
title: 多个提供程序的迁移-EF Core
description: 在将多个数据库提供程序定位到 Entity Framework Core 时使用迁移来管理数据库架构
author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: f44abb5156ea3a175c68c1a0ec23ff41a9d13452
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061978"
---
# <a name="migrations-with-multiple-providers"></a>迁移多个提供程序

[EF Core 工具][1]仅基架活动提供程序的迁移。 但有时，您可能希望使用多个提供程序 (例如 Microsoft SQL Server 和 SQLite) DbContext。 可以通过两种方式来处理迁移。 您可以维护两组迁移-每个提供程序一个，或将它们合并到可以同时使用的单个集。

## <a name="two-migration-sets"></a>两个迁移集

第一种方法是为每个模型更改生成两个迁移。

实现此目的的一种方法是将每个迁移集放 [在单独的程序集中][2] ，并在添加两个迁移之间手动切换活动提供程序 (和迁移程序集) 。

更轻松地使用工具的另一种方法是创建一个从 DbContext 派生的新类型并重写活动提供程序。 此类型在设计时用于添加或应用迁移。

```csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> 由于每个迁移集使用自己的 DbContext 类型，因此此方法不需要使用单独的迁移程序集。

添加新迁移时，指定上下文类型。

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> 无需指定输出目录即可进行后续迁移，因为它们是以同级方式创建的。

## <a name="one-migration-set"></a>一个迁移集

如果不喜欢两组迁移，可以手动将它们合并为可应用于这两个提供程序的单个集。

批注可以共存，因为提供程序会忽略它不理解的任何批注。 例如，使用 Microsoft SQL Server 和 SQLite 的主键列可能如下所示。

```csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

如果只能对一个提供程序应用操作，或它们在提供程序之间不同，请使用 `ActiveProvider` 属性来确定哪个提供程序处于活动状态：

```csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: xref:core/miscellaneous/cli/index
  [2]: xref:core/managing-schemas/migrations/projects
