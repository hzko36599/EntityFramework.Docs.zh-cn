---
title: 实体类型-EF Core
description: 如何使用 Entity Framework Core 配置和映射实体类型
author: roji
ms.date: 10/06/2020
uid: core/modeling/entity-types
ms.openlocfilehash: ca8cb8560afe374218e763bc0476839187a40ece
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635765"
---
# <a name="entity-types"></a>实体类型

在上下文中包含一种类型的 DbSet，这意味着它包含在 EF Core 的模型中;我们通常将此类类型称为 *实体*。 EF Core 可以从/向数据库中读取和写入实体实例，如果使用的是关系数据库，EF Core 可以通过迁移为实体创建表。

## <a name="including-types-in-the-model"></a>在模型中包含类型

按照约定，在上下文中的 DbSet 属性中公开的类型作为实体包含在模型中。 还包括方法中指定的实体类型 `OnModelCreating` ，就像通过递归方式浏览其他发现的实体类型的导航属性找到的任何类型一样。

在下面的代码示例中，包含了所有类型：

* `Blog` 包含，因为它在上下文的 DbSet 属性中公开。
* `Post` 包含，因为它通过 `Blog.Posts` 导航属性发现。
* `AuditEntry` 因为它是在中指定的 `OnModelCreating` 。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>从模型中排除类型

如果您不希望在模型中包含某一类型，则可以排除它：

### <a name="data-annotations"></a>[数据批注](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

### <a name="excluding-from-migrations"></a>从迁移中排除

> [!NOTE]
> EF Core 5.0 中引入了从迁移中排除表的功能。

有时在多个类型中映射相同的实体类型会很有用 `DbContext` 。 当使用 [绑定上下文](https://www.martinfowler.com/bliki/BoundedContext.html)时，尤其是对于 `DbContext` 每个边界上下文都有不同类型的情况。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableExcludeFromMigrations.cs?name=TableExcludeFromMigrations&highlight=4)]

此配置迁移不会创建 `AspNetUsers` 该表，但 `IdentityUser` 仍包含在模型中，并且可正常使用。

如果需要再次使用迁移来管理表，则应创建不包括的新迁移 `AspNetUsers` 。 下一次迁移将包含对表所做的任何更改。

## <a name="table-name"></a>表名

按照约定，每个实体类型将设置为映射到与公开实体的 DbSet 属性同名的数据库表。 如果给定实体不存在 DbSet，则使用类名称。

可以手动配置表名：

### <a name="data-annotations"></a>[数据批注](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

**_

## <a name="table-schema"></a>表架构

使用关系数据库时，表按约定在数据库的默认架构中创建。 例如，Microsoft SQL Server 将使用 `dbo` 架构 (SQLite 不支持) 的架构。

你可以配置要在特定架构中创建的表，如下所示：

### <a name="data-annotations"></a>[数据批注](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

_*_

您还可以在模型级别定义 Fluent API 的默认架构，而不是为每个表指定架构：

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

请注意，设置默认架构也会影响其他数据库对象，如序列。

## <a name="view-mapping"></a>查看映射

实体类型可以使用熟知的 API 映射到数据库视图。

> [!Note]
> EF 假设数据库中已存在引用的视图，它不会在迁移中自动创建它。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ViewNameAndSchema.cs?name=ViewNameAndSchema&highlight=1)]

 映射到视图将删除默认表映射，但从 EF 5.0 开始，实体类型也可以显式映射到表。 在这种情况下，查询映射将用于查询，表映射将用于更新。

> [!TIP]
> 若要测试使用内存中提供程序映射到视图的实体类型，请通过将它们映射到查询 `ToInMemoryQuery` 。 有关更多详细信息，请参阅使用此技术的可 [运行示例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/) 。

## <a name="table-valued-function-mapping"></a>表值函数映射

可以将实体类型映射到表值函数 (TVF) 而不是数据库中的表。 为了说明这一点，让我们定义另一个实体来表示具有多个帖子的博客。 在此示例中，实体是 [无键](xref:core/modeling/keyless-entity-types)的，但不一定要这样做。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs#BlogWithMultiplePostsEntity)]

接下来，在数据库中创建以下表值函数，该函数将仅返回包含多个帖子的博客以及与其中每个博客关联的帖子数：

```sql
CREATE FUNCTION dbo.BlogsWithMultiplePosts()
RETURNS TABLE
AS
RETURN
(
    SELECT b.Url, COUNT(p.BlogId) AS PostCount
    FROM Blogs AS b
    JOIN Posts AS p ON b.BlogId = p.BlogId
    GROUP BY b.BlogId, b.Url
    HAVING COUNT(p.BlogId) > 1
)
```

现在， `BlogWithMultiplePost` 可以通过以下方式将实体映射到此函数：

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs#QueryableFunctionConfigurationToFunction)]

> [!NOTE]
> 若要将实体映射到表值函数，该函数必须是无参数的。

通常，实体属性将映射到 TVF 返回的匹配列。 如果 TVF 返回的列的名称与实体属性的名称不同，则可以使用方法对其进行配置 `HasColumnName` ，就像映射到常规表一样。

如果实体类型映射到表值函数，则查询：

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Program.cs#ToFunctionQuery)]

生成以下 SQL：

```sql
SELECT [b].[Url], [b].[PostCount]
FROM [dbo].[BlogsWithMultiplePosts]() AS [b]
WHERE [b].[PostCount] > 3
```

## <a name="table-comments"></a>表注释

您可以设置对数据库表设置的任意文本注释，以允许您在数据库中记录架构：

### <a name="data-annotations"></a>[数据批注](#tab/data-annotations)

> [!NOTE]
> EF Core 5.0 中引入了通过数据批注设置注释。

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableComment.cs?name=TableComment&highlight=1)]

### <a name="fluent-api"></a>[Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableComment.cs?name=TableComment&highlight=4)]

_**
