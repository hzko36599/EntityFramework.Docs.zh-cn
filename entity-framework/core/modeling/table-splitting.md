---
title: 表拆分-EF Core
description: 如何使用 Entity Framework Core 配置表拆分
author: AndriySvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: 71d332ec3f500f48e12863c71ac44ce4d60699f0
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063031"
---
# <a name="table-splitting"></a><span data-ttu-id="7b8d6-103">表拆分</span><span class="sxs-lookup"><span data-stu-id="7b8d6-103">Table Splitting</span></span>

<span data-ttu-id="7b8d6-104">EF Core 允许将两个或多个实体映射到单个行。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="7b8d6-105">这称为 " _表拆分_ " 或 " _表共享_"。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="7b8d6-106">配置</span><span class="sxs-lookup"><span data-stu-id="7b8d6-106">Configuration</span></span>

<span data-ttu-id="7b8d6-107">若要使用表拆分，则需要将多个实体类型映射到同一个表中，将主键映射到相同的列，并且在同一个实体类型的主键与另一个实体类型的主键之间配置至少一个关系。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="7b8d6-108">表拆分的常见方案是只使用表中的部分列，以获得更好的性能或封装。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="7b8d6-109">在此示例中， `Order` 表示的子集 `DetailedOrder` 。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="7b8d6-110">除了所需的配置之外，我们还调用 `Property(o => o.Status).HasColumnName("Status")` 将映射 `DetailedOrder.Status` 到与相同的列 `Order.Status` 。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="7b8d6-111">有关更多上下文，请参阅 [完整的示例项目](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) 。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-111">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="7b8d6-112">使用情况</span><span class="sxs-lookup"><span data-stu-id="7b8d6-112">Usage</span></span>

<span data-ttu-id="7b8d6-113">使用表拆分来保存和查询实体的方式与其他实体相同：</span><span class="sxs-lookup"><span data-stu-id="7b8d6-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="7b8d6-114">可选依赖实体</span><span class="sxs-lookup"><span data-stu-id="7b8d6-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="7b8d6-115">EF Core 3.0 中引入了此功能。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="7b8d6-116">如果依赖实体使用的所有列都 `NULL` 在数据库中，则查询时不会创建它的实例。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="7b8d6-117">这允许对一个可选的依赖实体建模，其中主体的关系属性将为 null。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="7b8d6-118">请注意，如果所有依赖属性均为可选且设置为，则会发生这种情况 `null` ，这可能不是预期的。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-118">Note that this would also happen if all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="7b8d6-119">并发标记</span><span class="sxs-lookup"><span data-stu-id="7b8d6-119">Concurrency tokens</span></span>

<span data-ttu-id="7b8d6-120">如果共享表的任何实体类型都有并发标记，则还必须将其包含在所有其他实体类型中。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="7b8d6-121">当只更新映射到同一个表中的一个实体时，必须使用此值来避免陈旧并发令牌值。</span><span class="sxs-lookup"><span data-stu-id="7b8d6-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="7b8d6-122">若要避免向使用代码公开并发标记，可以将其创建为 [影子属性](xref:core/modeling/shadow-properties)：</span><span class="sxs-lookup"><span data-stu-id="7b8d6-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
