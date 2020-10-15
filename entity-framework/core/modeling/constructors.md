---
title: 具有构造函数的实体类型-EF Core
description: 使用构造函数将数据与 Entity Framework Core 模型绑定
author: ajcvickers
ms.date: 02/23/2018
uid: core/modeling/constructors
ms.openlocfilehash: 9502d75072eebb80c37cf1805e21f7d112269ba1
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063707"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="9cd26-103">具有构造函数的实体类型</span><span class="sxs-lookup"><span data-stu-id="9cd26-103">Entity types with constructors</span></span>

<span data-ttu-id="9cd26-104">可以使用参数定义构造函数，并在创建实体实例时使 EF Core 调用此构造函数。</span><span class="sxs-lookup"><span data-stu-id="9cd26-104">It's possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="9cd26-105">构造函数参数可以绑定到映射的属性或各种类型的服务，以促进延迟加载等行为。</span><span class="sxs-lookup"><span data-stu-id="9cd26-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]
> <span data-ttu-id="9cd26-106">目前，所有构造函数绑定都按约定进行。</span><span class="sxs-lookup"><span data-stu-id="9cd26-106">Currently, all constructor binding is by convention.</span></span> <span data-ttu-id="9cd26-107">计划在将来的版本中配置要使用的特定构造函数。</span><span class="sxs-lookup"><span data-stu-id="9cd26-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="9cd26-108">绑定到映射的属性</span><span class="sxs-lookup"><span data-stu-id="9cd26-108">Binding to mapped properties</span></span>

<span data-ttu-id="9cd26-109">请考虑典型的博客/公告模型：</span><span class="sxs-lookup"><span data-stu-id="9cd26-109">Consider a typical Blog/Post model:</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="9cd26-110">当 EF Core 创建这些类型的实例（例如，对于查询结果）时，它将首先调用默认的无参数构造函数，然后将每个属性设置为数据库中的值。</span><span class="sxs-lookup"><span data-stu-id="9cd26-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="9cd26-111">但是，如果 EF Core 查找参数名称和类型与映射的属性相匹配的参数化构造函数，则将改为使用这些属性的值调用参数化构造函数，并且不会显式设置每个属性。</span><span class="sxs-lookup"><span data-stu-id="9cd26-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="9cd26-112">例如：</span><span class="sxs-lookup"><span data-stu-id="9cd26-112">For example:</span></span>

```csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="9cd26-113">需要注意的事项：</span><span class="sxs-lookup"><span data-stu-id="9cd26-113">Some things to note:</span></span>

* <span data-ttu-id="9cd26-114">并非所有属性都需要具有构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="9cd26-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="9cd26-115">例如，后期内容属性不是由任何构造函数参数设置的，因此 EF Core 会在以正常方式调用构造函数后对其进行设置。</span><span class="sxs-lookup"><span data-stu-id="9cd26-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="9cd26-116">参数类型和名称必须与属性类型和名称相匹配，但在参数采用 camel 大小写格式时，属性可以采用 Pascal 大小写形式。</span><span class="sxs-lookup"><span data-stu-id="9cd26-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="9cd26-117">EF Core 无法使用构造函数在) 以上的博客或文章 (设置导航属性。</span><span class="sxs-lookup"><span data-stu-id="9cd26-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="9cd26-118">构造函数可以是公共的，也可以是私有的，或者具有任何其他可访问性。</span><span class="sxs-lookup"><span data-stu-id="9cd26-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="9cd26-119">不过，延迟加载代理要求构造函数可从继承代理类访问。</span><span class="sxs-lookup"><span data-stu-id="9cd26-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="9cd26-120">通常，这意味着将其设为公共或受保护。</span><span class="sxs-lookup"><span data-stu-id="9cd26-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="9cd26-121">只读属性</span><span class="sxs-lookup"><span data-stu-id="9cd26-121">Read-only properties</span></span>

<span data-ttu-id="9cd26-122">通过构造函数设置属性后，就可以使某些属性成为只读的。</span><span class="sxs-lookup"><span data-stu-id="9cd26-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="9cd26-123">EF Core 支持此功能，但需要注意以下事项：</span><span class="sxs-lookup"><span data-stu-id="9cd26-123">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="9cd26-124">不会按约定映射没有 setter 的属性。</span><span class="sxs-lookup"><span data-stu-id="9cd26-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="9cd26-125"> (这样做通常是为了映射不应映射的属性，如计算属性。 ) </span><span class="sxs-lookup"><span data-stu-id="9cd26-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="9cd26-126">使用自动生成的键值需要键属性，该属性是读写的，因为在插入新实体时密钥生成器需要设置密钥值。</span><span class="sxs-lookup"><span data-stu-id="9cd26-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="9cd26-127">避免这种情况的简单方法是使用专用资源库。</span><span class="sxs-lookup"><span data-stu-id="9cd26-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="9cd26-128">例如：</span><span class="sxs-lookup"><span data-stu-id="9cd26-128">For example:</span></span>

```csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="9cd26-129">EF Core 会看到一个具有专用资源库的属性为读写属性，这意味着所有属性都将像以前一样映射，并且该密钥仍可存储生成。</span><span class="sxs-lookup"><span data-stu-id="9cd26-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="9cd26-130">使用专用资源库的一种替代方法是使属性真正为只读，并在 OnModelCreating 中添加更多显式映射。</span><span class="sxs-lookup"><span data-stu-id="9cd26-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="9cd26-131">同样，可以完全删除某些属性并仅替换为字段。</span><span class="sxs-lookup"><span data-stu-id="9cd26-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="9cd26-132">以下面这些实体类型为例：</span><span class="sxs-lookup"><span data-stu-id="9cd26-132">For example, consider these entity types:</span></span>

```csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="9cd26-133">OnModelCreating 中的此配置：</span><span class="sxs-lookup"><span data-stu-id="9cd26-133">And this configuration in OnModelCreating:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```

<span data-ttu-id="9cd26-134">注意事项：</span><span class="sxs-lookup"><span data-stu-id="9cd26-134">Things to note:</span></span>

* <span data-ttu-id="9cd26-135">"属性" 键现在是字段。</span><span class="sxs-lookup"><span data-stu-id="9cd26-135">The key "property" is now a field.</span></span> <span data-ttu-id="9cd26-136">它不是一个 `readonly` 字段，因此可以使用存储生成的键。</span><span class="sxs-lookup"><span data-stu-id="9cd26-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="9cd26-137">其他属性是仅在构造函数中设置的只读属性。</span><span class="sxs-lookup"><span data-stu-id="9cd26-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="9cd26-138">如果主键值只是由 EF 设置或从数据库中读取，则无需将其包含在构造函数中。</span><span class="sxs-lookup"><span data-stu-id="9cd26-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="9cd26-139">这会使 "属性" 键作为一个简单字段，并清楚地指出不应在创建新的博客或文章时显式设置。</span><span class="sxs-lookup"><span data-stu-id="9cd26-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]
> <span data-ttu-id="9cd26-140">此代码将导致编译器警告 "169"，指示该字段从未使用过。</span><span class="sxs-lookup"><span data-stu-id="9cd26-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="9cd26-141">这可能会被忽略，因为在现实中 EF Core 以 extralinguistic 的方式使用该字段。</span><span class="sxs-lookup"><span data-stu-id="9cd26-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="9cd26-142">注入服务</span><span class="sxs-lookup"><span data-stu-id="9cd26-142">Injecting services</span></span>

<span data-ttu-id="9cd26-143">EF Core 还可以将 "服务" 注入到实体类型的构造函数中。</span><span class="sxs-lookup"><span data-stu-id="9cd26-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="9cd26-144">例如，可以注入以下内容：</span><span class="sxs-lookup"><span data-stu-id="9cd26-144">For example, the following can be injected:</span></span>

* <span data-ttu-id="9cd26-145">`DbContext` -当前上下文实例，也可以类型化为派生的 DbContext 类型</span><span class="sxs-lookup"><span data-stu-id="9cd26-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="9cd26-146">`ILazyLoader`-延迟加载服务-有关更多详细信息，请参阅[延迟加载文档](xref:core/querying/related-data)</span><span class="sxs-lookup"><span data-stu-id="9cd26-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](xref:core/querying/related-data) for more details</span></span>
* <span data-ttu-id="9cd26-147">`Action<object, string>`-延迟加载委托-有关更多详细信息，请参阅[延迟加载文档](xref:core/querying/related-data)</span><span class="sxs-lookup"><span data-stu-id="9cd26-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](xref:core/querying/related-data) for more details</span></span>
* <span data-ttu-id="9cd26-148">`IEntityType` -与此实体类型关联的 EF Core 元数据</span><span class="sxs-lookup"><span data-stu-id="9cd26-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]
> <span data-ttu-id="9cd26-149">目前，只能注入 EF Core 已知的服务。</span><span class="sxs-lookup"><span data-stu-id="9cd26-149">Currently, only services known by EF Core can be injected.</span></span> <span data-ttu-id="9cd26-150">将来的版本会考虑对注入应用程序服务的支持。</span><span class="sxs-lookup"><span data-stu-id="9cd26-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="9cd26-151">例如，注入的 DbContext 可用于有选择地访问数据库，以获取相关实体的相关信息，而无需将它们全部加载。</span><span class="sxs-lookup"><span data-stu-id="9cd26-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="9cd26-152">在下面的示例中，这用于在不加载帖子的情况下获取博客中的帖子数：</span><span class="sxs-lookup"><span data-stu-id="9cd26-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="9cd26-153">请注意以下几点：</span><span class="sxs-lookup"><span data-stu-id="9cd26-153">A few things to notice about this:</span></span>

* <span data-ttu-id="9cd26-154">构造函数是私有的，因为它只是由 EF Core 调用的，另一个公共构造函数用于常规用途。</span><span class="sxs-lookup"><span data-stu-id="9cd26-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="9cd26-155">使用注入的服务的代码 (即，上下文) 防御 `null` 处理 EF Core 不创建实例的情况。</span><span class="sxs-lookup"><span data-stu-id="9cd26-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="9cd26-156">因为服务存储在读/写属性中，所以当将实体附加到新的上下文实例时，它将被重置。</span><span class="sxs-lookup"><span data-stu-id="9cd26-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]
> <span data-ttu-id="9cd26-157">注入此类 DbContext 通常被视为一种反模式，因为它会将您的实体类型直接耦合到 EF Core。</span><span class="sxs-lookup"><span data-stu-id="9cd26-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="9cd26-158">使用此类服务注入之前，请仔细考虑所有选项。</span><span class="sxs-lookup"><span data-stu-id="9cd26-158">Carefully consider all options before using service injection like this.</span></span>
