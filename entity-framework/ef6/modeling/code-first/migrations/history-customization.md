---
title: 自定义迁移历史记录表-EF6
description: 自定义实体框架6中的迁移历史记录表
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/migrations/history-customization
ms.openlocfilehash: f0f474507659d5fbe43ecf2ed7216fb5d1a86779
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066390"
---
# <a name="customizing-the-migrations-history-table"></a>自定义迁移历史记录表
> [!NOTE]
> **仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。 如果使用的是早期版本，则部分或全部信息不适用。

> [!NOTE]
> 本文假设你知道如何在基本方案中使用 Code First 迁移。 如果没有，则需要先阅读 [Code First 迁移](xref:ef6/modeling/code-first/migrations/index) ，然后再继续。

## <a name="what-is-migrations-history-table"></a>什么是迁移历史记录表？

迁移历史记录表是 Code First 迁移用来存储应用于数据库的迁移的详细信息的表。 默认情况下，数据库中的表的名称为 \_ \_ MigrationHistory，并且在应用第一次迁移到数据库时创建该名称。 在实体框架5中，如果应用程序使用 Microsoft Sql Server 数据库，则此表为系统表。 这在实体框架6中已更改，并且迁移历史记录表不再标记为系统表。

## <a name="why-customize-migrations-history-table"></a>为什么自定义迁移历史记录表？

迁移历史记录表应仅由 Code First 迁移使用，并可以手动更改，从而中断迁移。 但有时，默认配置不合适，需要对表进行自定义，例如：

-   需要更改列的名称和/或 facet，以启用3个<sup>rd</sup> 方迁移提供程序
-   要更改表的名称
-   需要对 \_ MigrationHistory 表使用非默认架构 \_
-   需要为特定版本的上下文存储其他数据，因此需要向表中添加其他列

## <a name="words-of-precaution"></a>预防词

更改迁移历史记录表的功能非常强大，但需小心不不要过度编写。 EF 运行时当前不检查自定义迁移历史记录表是否与运行时兼容。 如果不是，你的应用程序可能会在运行时中断或以不可预知的方式运行。 如果对每个数据库使用多个上下文，这一点更重要，在这种情况下，多个上下文可以使用相同的迁移历史记录表来存储有关迁移的信息。

## <a name="how-to-customize-migrations-history-table"></a>如何自定义迁移历史记录表？

在开始之前，你需要知道只能在应用首次迁移之前自定义迁移历史记录表。 现在，到代码。

首先，你将需要创建一个派生自 HistoryContext 类的类，。 HistoryContext 类派生自 DbContext 类，因此配置迁移历史记录表非常类似于配置 EF 模型与 Fluent API。 只需重写 OnModelCreating 方法并使用 Fluent API 来配置该表。

>[!NOTE]
> 通常，在配置 EF 模型时，无需调用 base。OnModelCreating 从重写的 OnModelCreating 方法 ( # A1，因为 OnModelCreating ( # A3 包含空正文。 配置迁移历史记录表时不会出现这种情况。 在这种情况下，需要在 OnModelCreating 中执行的第一件事 ( # A1 替代是实际调用 base。OnModelCreating ( # A3。 这会将迁移历史记录表配置为默认方法，然后在重写方法中进行调整。

假设你想要重命名迁移历史记录表，并将其放入名为 "admin" 的自定义架构。 此外，DBA 需要将 MigrationId 列重命名为 "迁移 ID" \_ 。 可以通过创建以下派生自 HistoryContext 的类来实现此目的：

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

自定义 HistoryContext 准备就绪后，你需要通过 [基于代码的配置](https://msdn.com/data/jj680699)注册 EF 来了解它：

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

就这么多了。 现在，你可以前往包管理器控制台，启用-迁移，Add-Migration，最后更新数据库。 这会导致将迁移历史记录表添加到数据库，并根据你在 HistoryContext 派生类中指定的详细信息进行配置。

![迁移历史记录表](~/ef6/media/database.png)
