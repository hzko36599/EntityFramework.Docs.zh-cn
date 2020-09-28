---
title: Azure Cosmos DB Provider - EF Core
description: 数据库提供程序的文档，该提供程序允许将 Entity Framework Core 与 Azure Cosmos DB SQL API 一起使用
author: AndriySvyryd
ms.date: 09/14/2020
uid: core/providers/cosmos/index
ms.openlocfilehash: 94ba29f3f2643e8f563a460e17dce9d15cb7c2df
ms.sourcegitcommit: c0e6a00b64c2dcd8acdc0fe6d1b47703405cdf09
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91210336"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="60730-103">EF Core Azure Cosmos DB Provider</span><span class="sxs-lookup"><span data-stu-id="60730-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="60730-104">此提供程序是 EF Core 3.0 新增内容。</span><span class="sxs-lookup"><span data-stu-id="60730-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="60730-105">此数据库提供程序允许将 Entity Framework Core 与 Azure Cosmos DB 一起使用。</span><span class="sxs-lookup"><span data-stu-id="60730-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="60730-106">该提供程序作为 [Entity Framework Core 项目](https://github.com/aspnet/EntityFrameworkCore)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="60730-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="60730-107">在阅读本部分之前，强烈建议先熟悉 [Azure Cosmos DB 文档](/azure/cosmos-db/introduction)。</span><span class="sxs-lookup"><span data-stu-id="60730-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="60730-108">此提供程序仅适用于 Azure Cosmos DB 的 SQL API。</span><span class="sxs-lookup"><span data-stu-id="60730-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="60730-109">安装</span><span class="sxs-lookup"><span data-stu-id="60730-109">Install</span></span>

<span data-ttu-id="60730-110">安装 [Microsoft.EntityFrameworkCore.Cosmos NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)。</span><span class="sxs-lookup"><span data-stu-id="60730-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="60730-111">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="60730-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[<span data-ttu-id="60730-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60730-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="60730-113">入门</span><span class="sxs-lookup"><span data-stu-id="60730-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="60730-114">可在 [GitHub 示例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos)中查看此文章的示例。</span><span class="sxs-lookup"><span data-stu-id="60730-114">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="60730-115">与其他提供程序一样，第一步是调用 [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)：</span><span class="sxs-lookup"><span data-stu-id="60730-115">As for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="60730-116">为了简单起见，此处对终结点和密钥进行了硬编码，但在生产应用中，应[安全地存储](/aspnet/core/security/app-secrets#secret-manager)这些终结点和密钥。</span><span class="sxs-lookup"><span data-stu-id="60730-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securely](/aspnet/core/security/app-secrets#secret-manager).</span></span>

<span data-ttu-id="60730-117">在此示例中，`Order` 是一个简单实体，其中包含对[从属类型](xref:core/modeling/owned-entities) `StreetAddress` 的引用。</span><span class="sxs-lookup"><span data-stu-id="60730-117">In this example `Order` is a simple entity with a reference to the [owned type](xref:core/modeling/owned-entities) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="60730-118">保存和查询数据遵循常规 EF 模式：</span><span class="sxs-lookup"><span data-stu-id="60730-118">Saving and querying data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="60730-119">要创建所需的容器并插入[种子数据](xref:core/modeling/data-seeding)（如果存在于模型中），则需要调用 [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync)。</span><span class="sxs-lookup"><span data-stu-id="60730-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](xref:core/modeling/data-seeding) if present in the model.</span></span> <span data-ttu-id="60730-120">但是只应在部署期间调用 `EnsureCreatedAsync`，而不应在正常操作中调用，否则可能会导致性能问题。</span><span class="sxs-lookup"><span data-stu-id="60730-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-options"></a><span data-ttu-id="60730-121">Cosmos 选项</span><span class="sxs-lookup"><span data-stu-id="60730-121">Cosmos options</span></span>

<span data-ttu-id="60730-122">还可以使用单个连接字符串配置 Cosmos DB 提供程序，并指定其他选项来自定义连接：</span><span class="sxs-lookup"><span data-stu-id="60730-122">It is also possible to configure the Cosmos DB provider with a single connection string and to specify other options to customize the connection:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OptionsContext.cs?name=Configuration)]

> [!NOTE]
> <span data-ttu-id="60730-123">其中大多数选项都是 EF Core Cosmos 5.0 中新增的选项。</span><span class="sxs-lookup"><span data-stu-id="60730-123">Most of these options are new in EF Core Cosmos 5.0.</span></span>

> [!TIP]
> <span data-ttu-id="60730-124">有关上述每种选项的效果的详细说明，请参阅 [Azure Cosmos DB 选项文档](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions)。</span><span class="sxs-lookup"><span data-stu-id="60730-124">See the [Azure Cosmos DB Options documentation](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions) for a detailed description of the effect of each option mentioned above.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="60730-125">特定于 Cosmos 的模型自定义</span><span class="sxs-lookup"><span data-stu-id="60730-125">Cosmos-specific model customization</span></span>

<span data-ttu-id="60730-126">默认情况下，所有实体类型都映射到同一个容器，该容器以派生的上下文命名（在本例中为 `"OrderContext"`）。</span><span class="sxs-lookup"><span data-stu-id="60730-126">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="60730-127">要更改默认容器名称，请使用 [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)：</span><span class="sxs-lookup"><span data-stu-id="60730-127">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="60730-128">要将实体类型映射到其他容器，请使用 [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)：</span><span class="sxs-lookup"><span data-stu-id="60730-128">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="60730-129">为了标识给定项表示的实体类型，EF Core 添加鉴别器值（即使没有派生实体类型）。</span><span class="sxs-lookup"><span data-stu-id="60730-129">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="60730-130">[可以更改](xref:core/modeling/inheritance)鉴别器的名称和值。</span><span class="sxs-lookup"><span data-stu-id="60730-130">The name and value of the discriminator [can be changed](xref:core/modeling/inheritance).</span></span>

<span data-ttu-id="60730-131">如果其他实体类型永远不会存储在同一个容器中，则可以通过调用 [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator) 删除鉴别器：</span><span class="sxs-lookup"><span data-stu-id="60730-131">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="60730-132">分区键</span><span class="sxs-lookup"><span data-stu-id="60730-132">Partition keys</span></span>

<span data-ttu-id="60730-133">默认情况下，EF Core 将创建分区键设置为 `"__partitionKey"` 的容器，而不会在插入项时为其提供任何值。</span><span class="sxs-lookup"><span data-stu-id="60730-133">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="60730-134">但若要充分利用 Azure Cosmos 的性能功能，[应仔细选择应使用的分区键](/azure/cosmos-db/partition-data)。</span><span class="sxs-lookup"><span data-stu-id="60730-134">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="60730-135">可以通过调用 [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)来配置它：</span><span class="sxs-lookup"><span data-stu-id="60730-135">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="60730-136">只要分区键属性[转换为字符串](xref:core/modeling/value-conversions)，则它可以为任意类型。</span><span class="sxs-lookup"><span data-stu-id="60730-136">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="60730-137">配置分区键属性后，应始终具有非 null 值。</span><span class="sxs-lookup"><span data-stu-id="60730-137">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="60730-138">发出查询时，可以添加条件将其设置为单分区。</span><span class="sxs-lookup"><span data-stu-id="60730-138">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="60730-139">嵌入的实体</span><span class="sxs-lookup"><span data-stu-id="60730-139">Embedded entities</span></span>

<span data-ttu-id="60730-140">对于 Cosmos，从属实体嵌入到所有者所在的项中。</span><span class="sxs-lookup"><span data-stu-id="60730-140">For Cosmos, owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="60730-141">要更改属性名称，请使用 [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)：</span><span class="sxs-lookup"><span data-stu-id="60730-141">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="60730-142">对于此配置，以上示例中的顺序存储如下：</span><span class="sxs-lookup"><span data-stu-id="60730-142">With this configuration the order from the example above is stored like this:</span></span>

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="60730-143">还嵌入了从属实体的集合。</span><span class="sxs-lookup"><span data-stu-id="60730-143">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="60730-144">对于下一个示例，我们将使用具有 `StreetAddress` 集合的 `Distributor` 类：</span><span class="sxs-lookup"><span data-stu-id="60730-144">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="60730-145">从属实体不需要提供要存储的显式键值：</span><span class="sxs-lookup"><span data-stu-id="60730-145">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="60730-146">它们将以这种方式持久保存：</span><span class="sxs-lookup"><span data-stu-id="60730-146">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

<span data-ttu-id="60730-147">在内部而言，EF Core 始终需要对所有被跟踪实体提供唯一键值。</span><span class="sxs-lookup"><span data-stu-id="60730-147">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="60730-148">默认情况下，为从属类型集合创建的主键包含指向所有者的外键属性和与 JSON 数组中的索引对应的 `int` 属性。</span><span class="sxs-lookup"><span data-stu-id="60730-148">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="60730-149">要检索这些值，可使用以下条目 API：</span><span class="sxs-lookup"><span data-stu-id="60730-149">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="60730-150">必要时，可更改从属实体类型的默认主键，但应显式提供键值。</span><span class="sxs-lookup"><span data-stu-id="60730-150">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="60730-151">使用断开连接的实体</span><span class="sxs-lookup"><span data-stu-id="60730-151">Working with disconnected entities</span></span>

<span data-ttu-id="60730-152">每个项都需要具有一个对于给定分区键唯一的 `id` 值。</span><span class="sxs-lookup"><span data-stu-id="60730-152">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="60730-153">默认情况下 EF Core 通过使用 '|' 作为分隔符串联鉴别器和主键值来生成值。</span><span class="sxs-lookup"><span data-stu-id="60730-153">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="60730-154">仅当实体进入 `Added` 状态时才生成键值。</span><span class="sxs-lookup"><span data-stu-id="60730-154">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="60730-155">如果[附加实体](xref:core/saving/disconnected-entities)在 .NET 类型上没有用于存储值的 `id` 属性，则这可能会导致问题。</span><span class="sxs-lookup"><span data-stu-id="60730-155">This might pose a problem when [attaching entities](xref:core/saving/disconnected-entities) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="60730-156">要解决此限制，可以手动创建并设置 `id` 值，或者先将实体标记为已添加，然后将其更改为所需状态：</span><span class="sxs-lookup"><span data-stu-id="60730-156">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="60730-157">生成的 JSON 如下：</span><span class="sxs-lookup"><span data-stu-id="60730-157">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```
