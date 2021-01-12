---
title: 用户定义的函数映射 - EF Core
description: 将用户定义的函数映射到数据库函数
author: maumar
ms.date: 11/23/2020
uid: core/user-defined-function-mapping
ms.openlocfilehash: ba60abdc9c81b34b8f4ed8f501cf2f7e52ba9d7d
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657695"
---
# <a name="user-defined-function-mapping"></a><span data-ttu-id="33e7f-103">用户定义的函数映射</span><span class="sxs-lookup"><span data-stu-id="33e7f-103">User-defined function mapping</span></span>

<span data-ttu-id="33e7f-104">EF Core 允许在查询中使用用户定义的 SQL 函数。</span><span class="sxs-lookup"><span data-stu-id="33e7f-104">EF Core allows for using user-defined SQL functions in queries.</span></span> <span data-ttu-id="33e7f-105">为此，需要在模型配置过程中将函数映射到 CLR 方法。</span><span class="sxs-lookup"><span data-stu-id="33e7f-105">To do that, the functions need to be mapped to a CLR method during model configuration.</span></span> <span data-ttu-id="33e7f-106">将 LINQ 查询转换为 SQL 时，将调用用户定义的函数，而不是它已映射到的 CLR 函数。</span><span class="sxs-lookup"><span data-stu-id="33e7f-106">When translating the LINQ query to SQL, the user-defined function is called instead of the CLR function it has been mapped to.</span></span>

## <a name="mapping-a-method-to-a-sql-function"></a><span data-ttu-id="33e7f-107">将方法映射到 SQL 函数</span><span class="sxs-lookup"><span data-stu-id="33e7f-107">Mapping a method to a SQL function</span></span>

<span data-ttu-id="33e7f-108">为了说明用户定义的函数映射的工作原理，让我们定义以下实体：</span><span class="sxs-lookup"><span data-stu-id="33e7f-108">To illustrate how user-defined function mapping work, let's define the following entities:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#Entities)]

<span data-ttu-id="33e7f-109">以及以下模型配置：</span><span class="sxs-lookup"><span data-stu-id="33e7f-109">And the following model configuration:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#EntityConfiguration)]

<span data-ttu-id="33e7f-110">博客可以有多篇帖文，每篇帖文可以有多条评论。</span><span class="sxs-lookup"><span data-stu-id="33e7f-110">Blog can have many posts and each post can have many comments.</span></span>

<span data-ttu-id="33e7f-111">接下来，根据博客 `Id`，创建用户定义的函数 `CommentedPostCountForBlog`，该函数将返回针对给定博客至少具有一条评论的帖文计数：</span><span class="sxs-lookup"><span data-stu-id="33e7f-111">Next, create the user-defined function `CommentedPostCountForBlog`, which returns the count of posts with at least one comment for a given blog, based on the blog `Id`:</span></span>

```sql
CREATE FUNCTION dbo.CommentedPostCountForBlog(@id int)
RETURNS int
AS
BEGIN
    RETURN (SELECT COUNT(*)
        FROM [Posts] AS [p]
        WHERE ([p].[BlogId] = @id) AND ((
            SELECT COUNT(*)
            FROM [Comments] AS [c]
            WHERE [p].[PostId] = [c].[PostId]) > 0));
END
```

<span data-ttu-id="33e7f-112">为了在 EF Core 中使用此函数，我们将定义以下 CLR 方法，并将其映射到用户定义的函数：</span><span class="sxs-lookup"><span data-stu-id="33e7f-112">To use this function in EF Core, we define the following CLR method, which we map to the user-defined function:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#BasicFunctionDefinition)]

<span data-ttu-id="33e7f-113">CLR 方法的主体并不重要。</span><span class="sxs-lookup"><span data-stu-id="33e7f-113">The body of the CLR method is not important.</span></span> <span data-ttu-id="33e7f-114">不会在客户端调用此方法，除非 EF Core 不能转换其参数。</span><span class="sxs-lookup"><span data-stu-id="33e7f-114">The method will not be invoked client-side, unless EF Core can't translate its arguments.</span></span> <span data-ttu-id="33e7f-115">如果可以转换参数，EF Core 只关心方法签名。</span><span class="sxs-lookup"><span data-stu-id="33e7f-115">If the arguments can be translated, EF Core only cares about the method signature.</span></span>

> [!NOTE]
> <span data-ttu-id="33e7f-116">在此示例中，方法是基于 `DbContext` 定义的，但也可以将其定义为其他类中的静态方法。</span><span class="sxs-lookup"><span data-stu-id="33e7f-116">In the example, the method is defined on `DbContext`, but it can also be defined as a static method inside other classes.</span></span>

<span data-ttu-id="33e7f-117">此函数定义现在可以与模型配置中用户定义的函数关联：</span><span class="sxs-lookup"><span data-stu-id="33e7f-117">This function definition can now be associated with user-defined function in the model configuration:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#BasicFunctionConfiguration)]

<span data-ttu-id="33e7f-118">默认情况下，EF Core 尝试将 CLR 函数映射到具有相同名称的用户定义的函数。</span><span class="sxs-lookup"><span data-stu-id="33e7f-118">By default, EF Core tries to map CLR function to a user-defined function with the same name.</span></span> <span data-ttu-id="33e7f-119">如果名称不同，可以使用 `HasName` 为要映射到的用户定义的函数提供正确的名称。</span><span class="sxs-lookup"><span data-stu-id="33e7f-119">If the names differ, we can use `HasName` to provide the correct name for the user-defined function we want to map to.</span></span>

<span data-ttu-id="33e7f-120">接下来，执行下面的查询：</span><span class="sxs-lookup"><span data-stu-id="33e7f-120">Now, executing the following query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#BasicQuery)]

<span data-ttu-id="33e7f-121">将生成此 SQL：</span><span class="sxs-lookup"><span data-stu-id="33e7f-121">Will produce this SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[Rating], [b].[Url]
FROM [Blogs] AS [b]
WHERE [dbo].[CommentedPostCountForBlog]([b].[BlogId]) > 1
```

## <a name="mapping-a-method-to-a-custom-sql"></a><span data-ttu-id="33e7f-122">将方法映射到自定义 SQL</span><span class="sxs-lookup"><span data-stu-id="33e7f-122">Mapping a method to a custom SQL</span></span>

<span data-ttu-id="33e7f-123">EF Core 还允许将用户定义的函数转换为特定 SQL。</span><span class="sxs-lookup"><span data-stu-id="33e7f-123">EF Core also allows for user-defined functions that get converted to a specific SQL.</span></span> <span data-ttu-id="33e7f-124">SQL 表达式是在配置用户定义的函数过程中使用 `HasTranslation` 方法提供的。</span><span class="sxs-lookup"><span data-stu-id="33e7f-124">The SQL expression is provided using `HasTranslation` method during user-defined function configuration.</span></span>

<span data-ttu-id="33e7f-125">在下面的示例中，我们将创建一个函数，用于计算两个整数之间的百分比差异。</span><span class="sxs-lookup"><span data-stu-id="33e7f-125">In the example below, we'll create a function that computes percentage difference between two integers.</span></span>

<span data-ttu-id="33e7f-126">CLR 方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="33e7f-126">The CLR method is as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#HasTranslationFunctionDefinition)]

<span data-ttu-id="33e7f-127">函数定义如下：</span><span class="sxs-lookup"><span data-stu-id="33e7f-127">The function definition is as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#HasTranslationFunctionConfiguration)]

<span data-ttu-id="33e7f-128">定义函数后，可在查询中使用函数。</span><span class="sxs-lookup"><span data-stu-id="33e7f-128">Once we define the function, it can be used in the query.</span></span> <span data-ttu-id="33e7f-129">EF Core 会根据从 HasTranslation 构造的 SQL 表达式树将方法主体直接转换为 SQL，而不是调用数据库函数。</span><span class="sxs-lookup"><span data-stu-id="33e7f-129">Instead of calling database function, EF Core will translate the method body directly into SQL based on the SQL expression tree constructed from the HasTranslation.</span></span> <span data-ttu-id="33e7f-130">下面的 LINQ 查询可以：</span><span class="sxs-lookup"><span data-stu-id="33e7f-130">The following LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#HasTranslationQuery)]

<span data-ttu-id="33e7f-131">生成以下 SQL：</span><span class="sxs-lookup"><span data-stu-id="33e7f-131">Produces the following SQL:</span></span>

```sql
SELECT 100 * (ABS(CAST([p].[BlogId] AS float) - 3) / ((CAST([p].[BlogId] AS float) + 3) / 2))
FROM [Posts] AS [p]
```

## <a name="mapping-a-queryable-function-to-a-table-valued-function"></a><span data-ttu-id="33e7f-132">将可查询函数映射到表值函数</span><span class="sxs-lookup"><span data-stu-id="33e7f-132">Mapping a queryable function to a table-valued function</span></span>

<span data-ttu-id="33e7f-133">EF Core 还支持映射到表值函数，方法是使用返回实体类型的 `IQueryable` 的用户定义的 CLR 方法并允许 EF Core 映射带参数的 TVF。</span><span class="sxs-lookup"><span data-stu-id="33e7f-133">EF Core also supports mapping to a table-valued function using a user-defined CLR method returning an `IQueryable` of entity types, allowing EF Core to map TVFs with parameters.</span></span> <span data-ttu-id="33e7f-134">此过程类似于将标量用户定义的函数映射到 SQL 函数：我们需要数据库中的 TVF、在 LINQ 查询中使用的 CLR 函数，以及两者之间的映射。</span><span class="sxs-lookup"><span data-stu-id="33e7f-134">The process is similar to mapping a scalar user-defined function to a SQL function: we need a TVF in the database, a CLR function that is used in the LINQ queries, and a mapping between the two.</span></span>

<span data-ttu-id="33e7f-135">例如，我们将使用表值函数返回至少具有一个符合给定“赞”阈值的评论的所有帖文：</span><span class="sxs-lookup"><span data-stu-id="33e7f-135">As an example, we'll use a table-valued function that returns all posts having at least one comment that meets a given "Like" threshold:</span></span>

```sql
CREATE FUNCTION dbo.PostsWithPopularComments(@likeThreshold int)
RETURNS TABLE
AS
RETURN
(
    SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
    FROM [Posts] AS [p]
    WHERE (
        SELECT COUNT(*)
        FROM [Comments] AS [c]
        WHERE ([p].[PostId] = [c].[PostId]) AND ([c].[Likes] >= @likeThreshold)) > 0
)
```

<span data-ttu-id="33e7f-136">CLR 方法签名如下所示：</span><span class="sxs-lookup"><span data-stu-id="33e7f-136">The CLR method signature is as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#QueryableFunctionDefinition)]

> [!TIP]
> <span data-ttu-id="33e7f-137">CLR 函数主体中的 `FromExpression` 调用允许使用函数而不是常规 DbSet。</span><span class="sxs-lookup"><span data-stu-id="33e7f-137">The `FromExpression` call in the CLR function body allows for the function to be used instead of a regular DbSet.</span></span>

<span data-ttu-id="33e7f-138">下面是映射：</span><span class="sxs-lookup"><span data-stu-id="33e7f-138">And below is the mapping:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#QueryableFunctionConfigurationHasDbFunction)]

> [!CAUTION]
> <span data-ttu-id="33e7f-139">在[问题 23408](https://github.com/dotnet/efcore/issues/23408) 得到修复之前，映射到实体类型 `IQueryable` 时将替代 DbSet 的表的默认映射。</span><span class="sxs-lookup"><span data-stu-id="33e7f-139">Until [issue 23408](https://github.com/dotnet/efcore/issues/23408) is fixed, mapping to an `IQueryable` of entity types overrides the default mapping to a table for the DbSet.</span></span> <span data-ttu-id="33e7f-140">如有必要（例如，不是无键实体时），必须使用 `ToTable` 方法显式指定表映射。</span><span class="sxs-lookup"><span data-stu-id="33e7f-140">If necessary - for example when the entity is not keyless - mapping to the table must be specified explicitly using `ToTable` method.</span></span>

> [!NOTE]
> <span data-ttu-id="33e7f-141">可查询函数必须映射到表值函数，并且不能使用 `HasTranslation`。</span><span class="sxs-lookup"><span data-stu-id="33e7f-141">Queryable function must be mapped to a table-valued function and can't use of `HasTranslation`.</span></span>

<span data-ttu-id="33e7f-142">映射函数后，请考虑以下查询：</span><span class="sxs-lookup"><span data-stu-id="33e7f-142">When the function is mapped, the following query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#TableValuedFunctionQuery)]

<span data-ttu-id="33e7f-143">生成：</span><span class="sxs-lookup"><span data-stu-id="33e7f-143">Produces:</span></span>

```sql
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [dbo].[PostsWithPopularComments](@likeThreshold) AS [p]
ORDER BY [p].[Rating]
```