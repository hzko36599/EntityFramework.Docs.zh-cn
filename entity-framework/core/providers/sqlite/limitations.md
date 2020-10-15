---
title: SQLite 数据库提供程序-限制-EF Core
description: 与其他提供程序相比 Entity Framework Core SQLite 数据库提供程序的限制
author: bricelam
ms.date: 09/24/2020
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 3d696474d401e8fd6c26a78067d292f0bb97a457
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062732"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core 数据库提供程序限制

SQLite 提供程序有很多迁移限制。 其中的大多数限制是由基础 SQLite 数据库引擎中的限制引起的，并不特定于 EF。

## <a name="modeling-limitations"></a>建模限制

公共关系库 (实体框架关系数据库提供程序共享，) 定义 Api，用于建模大多数关系数据库引擎所共有的概念。 SQLite 提供程序不支持其中几个概念。

* 架构
* 序列

## <a name="query-limitations"></a>查询限制

SQLite 本身并不支持以下数据类型。 EF Core 可以读取和写入这些类型的值，并且还支持) 的相等性 (查询 `where e.Property == value` 。 但其他操作（如比较和排序）将需要对客户端进行评估。

* DateTimeOffset
* 小数
* TimeSpan
* UInt64

`DateTimeOffset`建议使用 DateTime 值，而不是。 处理多个时区时，建议在保存之前将值转换为 UTC，然后将其转换回适当的时区。

`Decimal`类型提供了较高的精度。 但是，如果不需要该级别的精度，则建议改为使用 double。 您可以使用 [值转换器](xref:core/modeling/value-conversions) 在类中继续使用 decimal。

```csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>迁移限制

SQLite 数据库引擎不支持许多其他关系数据库所支持的架构操作。 如果尝试将不受支持的操作之一应用于 SQLite 数据库，则 `NotSupportedException` 会引发。

将尝试重新生成以执行特定操作。 重新生成仅可用于作为 EF Core 模型一部分的数据库项目。 如果数据库项目不是模型的一部分，例如，如果它是在迁移内手动创建的，则 `NotSupportedException` 仍会引发。

| 操作            | 是否支持？  | 需要版本 |
|:---------------------|:------------|:-----------------|
| AddCheckConstraint   | 重新生成 (✔)  | 5.0              |
| AddColumn            | ✔           |                  |
| AddForeignKey        | 重新生成 (✔)  | 5.0              |
| AddPrimaryKey        | 重新生成 (✔)  | 5.0              |
| AddUniqueConstraint  | 重新生成 (✔)  | 5.0              |
| AlterColumn          | 重新生成 (✔)  | 5.0              |
| CreateIndex          | ✔           |                  |
| CreateTable          | ✔           |                  |
| DropCheckConstraint  | 重新生成 (✔)  | 5.0              |
| DropColumn           | 重新生成 (✔)  | 5.0              |
| DropForeignKey       | 重新生成 (✔)  | 5.0              |
| DropIndex            | ✔           |                  |
| DropPrimaryKey       | 重新生成 (✔)  | 5.0              |
| DropTable            | ✔           |                  |
| DropUniqueConstraint | 重新生成 (✔)  | 5.0              |
| RenameColumn         | ✔           | 2.2              |
| RenameIndex          | 重新生成 (✔)  |                  |
| RenameTable          | ✔           |                  |
| EnsureSchema         | ✔ (的)    |                  |
| DropSchema           | ✔ (的)    |                  |
| 插入               | ✔           |                  |
| 更新               | ✔           |                  |
| 删除               | ✔           |                  |

### <a name="migrations-limitations-workaround"></a>迁移限制解决方法

通过在迁移中手动编写代码来执行重新生成，可以解决其中一些限制。 表重建涉及创建新表，将数据复制到新表，删除旧表，重命名新表。 你将需要使用 `Sql(string)` 方法来执行其中一些步骤。

有关更多详细信息，请参阅在 SQLite 文档中 [进行其他类型的表架构更改](https://sqlite.org/lang_altertable.html#otheralter) 。

### <a name="idempotent-script-limitations"></a>幂等脚本限制

与其他数据库不同，SQLite 不包含过程语言。 因此，无法生成幂等迁移脚本所需的 if-then 逻辑。

如果知道最后一个应用到数据库的迁移，则可从迁移到最新迁移的脚本生成一个脚本。

```dotnetcli
dotnet ef migrations script CurrentMigration
```

否则，建议使用 `dotnet ef database update` 来应用迁移。 从 EF Core 5.0 开始，在运行命令时，可以指定数据库文件。

```dotnetcli
dotnet ef database update --connection "Data Source=My.db"
```

## <a name="see-also"></a>另请参阅

* [Node.js 异步限制](/dotnet/standard/data/sqlite/async)
* [ADO.NET 限制](/dotnet/standard/data/sqlite/adonet-limitations)
