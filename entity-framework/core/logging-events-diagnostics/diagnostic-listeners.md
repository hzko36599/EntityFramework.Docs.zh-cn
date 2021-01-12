---
title: 使用诊断侦听器-EF Core
description: 使用 DiagnosticListener 进行 EF Core 诊断的全局使用
author: ajcvickers
ms.date: 10/16/2020
uid: core/logging-events-diagnostics/diagnostic-listeners
ms.openlocfilehash: afb80aa8f05f70761e423f58653f681938079858
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129260"
---
# <a name="using-diagnostic-listeners-in-ef-core"></a><span data-ttu-id="7dcd5-103">在 EF Core 中使用诊断侦听器</span><span class="sxs-lookup"><span data-stu-id="7dcd5-103">Using Diagnostic Listeners in EF Core</span></span>

> [!TIP]
> <span data-ttu-id="7dcd5-104">可以从 GitHub [下载此文章的示例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) 。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-104">You can [download this article's sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) from GitHub.</span></span>

<span data-ttu-id="7dcd5-105">诊断侦听器允许侦听当前 .NET 进程中发生的任何 EF Core 事件。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-105">Diagnostic listeners allow listening for any EF Core event that occurs in the current .NET process.</span></span> <span data-ttu-id="7dcd5-106"><xref:System.Diagnostics.DiagnosticListener>类是一种[跨 .net 的通用机制](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)的组成部分，用于从运行的应用程序中获取诊断信息。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-106">The <xref:System.Diagnostics.DiagnosticListener> class is a part of a [common mechanism across .NET](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) for obtaining diagnostic information from running applications.</span></span>

<span data-ttu-id="7dcd5-107">诊断侦听器不适合从单个 DbContext 实例获取事件。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-107">Diagnostic listeners are not suitable for getting events from a single DbContext instance.</span></span> <span data-ttu-id="7dcd5-108">EF Core [拦截](xref:core/logging-events-diagnostics/interceptors) 程序使用每个上下文注册提供对相同事件的访问。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-108">EF Core [interceptors](xref:core/logging-events-diagnostics/interceptors) provide access to the same events with per-context registration.</span></span>

<span data-ttu-id="7dcd5-109">诊断侦听器不能用于日志记录。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-109">Diagnostic listeners are not designed for logging.</span></span> <span data-ttu-id="7dcd5-110">请考虑使用 [简单日志记录](xref:core/logging-events-diagnostics/simple-logging) 或用于日志记录 [的日志记录](xref:core/logging-events-diagnostics/extensions-logging) 。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-110">Consider using [simple logging](xref:core/logging-events-diagnostics/simple-logging) or [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging) for logging.</span></span>

## <a name="example-observing-diagnostic-events"></a><span data-ttu-id="7dcd5-111">示例：观察诊断事件</span><span class="sxs-lookup"><span data-stu-id="7dcd5-111">Example: Observing diagnostic events</span></span>

<span data-ttu-id="7dcd5-112">解析 EF Core 事件的过程分为两个步骤。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-112">Resolving EF Core events is a two-step process.</span></span> <span data-ttu-id="7dcd5-113">首先， [](/dotnet/standard/events/observer-design-pattern) `DiagnosticListener` 必须创建其自身的观察程序：</span><span class="sxs-lookup"><span data-stu-id="7dcd5-113">First, an [observer](/dotnet/standard/events/observer-design-pattern) for `DiagnosticListener` itself must be created:</span></span>

<!--
public class DiagnosticObserver : IObserver<DiagnosticListener>
{
    public void OnCompleted()
        => throw new NotImplementedException();

    public void OnError(Exception error)
        => throw new NotImplementedException();

    public void OnNext(DiagnosticListener value)
    {
        if (value.Name == DbLoggerCategory.Name) // "Microsoft.EntityFrameworkCore"
        {
            value.Subscribe(new KeyValueObserver());
        }
    }
}
-->
[!code-csharp[DiagnosticObserver](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=DiagnosticObserver)]

<span data-ttu-id="7dcd5-114">`OnNext`方法查找来自 EF Core 的 DiagnosticListener。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-114">The `OnNext` method looks for the DiagnosticListener that comes from EF Core.</span></span> <span data-ttu-id="7dcd5-115">此侦听器的名称为 "Microsoft.entityframeworkcore"，可从类中获得， <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> 如所示。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-115">This listener has the name "Microsoft.EntityFrameworkCore", which can be obtained from the <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> class as shown.</span></span>

<span data-ttu-id="7dcd5-116">然后，必须在应用程序的方法中全局注册此观察程序 `Main` ：</span><span class="sxs-lookup"><span data-stu-id="7dcd5-116">This observer must then be registered globally, for example in the application's `Main` method:</span></span>

<!--
        DiagnosticListener.AllListeners.Subscribe(new DiagnosticObserver());
-->
[!code-csharp[RegisterDiagnosticListener](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=RegisterDiagnosticListener)]

<span data-ttu-id="7dcd5-117">其次，一旦找到 EF Core DiagnosticListener，就会创建一个新的键-值观察器来订阅实际 EF Core 事件。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-117">Second, once the EF Core DiagnosticListener is found, a new key-value observer is created to subscribe to the actual EF Core events.</span></span> <span data-ttu-id="7dcd5-118">例如：</span><span class="sxs-lookup"><span data-stu-id="7dcd5-118">For example:</span></span>

<!--
public class KeyValueObserver : IObserver<KeyValuePair<string, object>>
{
    public void OnCompleted()
        => throw new NotImplementedException();

    public void OnError(Exception error)
        => throw new NotImplementedException();

    public void OnNext(KeyValuePair<string, object> value)
    {
        if (value.Key == CoreEventId.ContextInitialized.Name)
        {
            var payload = (ContextInitializedEventData)value.Value;
            Console.WriteLine($"EF is initializing {payload.Context.GetType().Name} ");
        }

        if (value.Key == RelationalEventId.ConnectionOpening.Name)
        {
            var payload = (ConnectionEventData)value.Value;
            Console.WriteLine($"EF is opening a connection to {payload.Connection.ConnectionString} ");
        }
    }
}
-->
[!code-csharp[KeyValueObserver](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=KeyValueObserver)]

<span data-ttu-id="7dcd5-119">`OnNext`此方法是使用每个 EF Core 事件的键/值对来调用的。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-119">The `OnNext` method is this time called with a key/value pair for each EF Core event.</span></span> <span data-ttu-id="7dcd5-120">密钥是事件的名称，可以从以下项之一获得：</span><span class="sxs-lookup"><span data-stu-id="7dcd5-120">The key is the name of the event, which can be obtained from one of:</span></span>

* <span data-ttu-id="7dcd5-121"><xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> 对于所有 EF Core 数据库提供程序所共有的事件</span><span class="sxs-lookup"><span data-stu-id="7dcd5-121"><xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> for events common to all EF Core database providers</span></span>
* <span data-ttu-id="7dcd5-122"><xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> 对于所有关系数据库提供程序通用的事件</span><span class="sxs-lookup"><span data-stu-id="7dcd5-122"><xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> for events common to all relational database providers</span></span>
* <span data-ttu-id="7dcd5-123">特定于当前数据库提供程序的事件的类似类。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-123">A similar class for events specific to the current database provider.</span></span> <span data-ttu-id="7dcd5-124">例如， <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> 对于 SQL Server 提供程序。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-124">For example, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> for the SQL Server provider.</span></span>

<span data-ttu-id="7dcd5-125">键/值对的值是特定于事件的负载类型。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-125">The value of the key/value pair is a payload type specific to the event.</span></span> <span data-ttu-id="7dcd5-126">预期的负载类型在这些事件类中定义的每个事件上记录。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-126">The type of payload to expect is documented on each event defined in these event classes.</span></span>

<span data-ttu-id="7dcd5-127">例如，上面的代码处理 <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ContextInitialized> 和 <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId.ConnectionOpening> 事件。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-127">For example, the code above handles the <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ContextInitialized> and the <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId.ConnectionOpening> events.</span></span> <span data-ttu-id="7dcd5-128">对于其中的第一种，负载是 <xref:Microsoft.EntityFrameworkCore.Diagnostics.ContextInitializedEventData> 。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-128">For the first of these, the payload is <xref:Microsoft.EntityFrameworkCore.Diagnostics.ContextInitializedEventData>.</span></span> <span data-ttu-id="7dcd5-129">对于第二个，它是 <xref:Microsoft.EntityFrameworkCore.Diagnostics.ConnectionEventData> 。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-129">For the second, it is <xref:Microsoft.EntityFrameworkCore.Diagnostics.ConnectionEventData>.</span></span>

> [!TIP]
> <span data-ttu-id="7dcd5-130">在每个 EF Core 事件数据类中重写 ToString，以生成事件的等效日志消息。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-130">ToString is overridden in every EF Core event data class to generate the equivalent log message for the event.</span></span> <span data-ttu-id="7dcd5-131">例如，调用会 `ContextInitializedEventData.ToString` 生成 "Entity Framework Core 5.0.0，并使用提供程序" microsoft.entityframeworkcore "（选项为" 无 "）初始化" BlogsContext "。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-131">For example, calling `ContextInitializedEventData.ToString` generates "Entity Framework Core 5.0.0 initialized 'BlogsContext' using provider 'Microsoft.EntityFrameworkCore.Sqlite' with options: None".</span></span>

<span data-ttu-id="7dcd5-132">该 [示例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) 包含一个简单的控制台应用程序，该应用程序对博客数据库进行更改并输出遇到的诊断事件。</span><span class="sxs-lookup"><span data-stu-id="7dcd5-132">The [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) contains a simple console application that makes changes to the blogging database and prints out the diagnostic events encountered.</span></span>

<!--
    public static void Main()
    {
        #region RegisterDiagnosticListener
        DiagnosticListener.AllListeners.Subscribe(new DiagnosticObserver());
        #endregion

        using (var context = new BlogsContext())
        {
            context.Database.EnsureDeleted();
            context.Database.EnsureCreated();

            context.Add(
                new Blog
                {
                    Name = "EF Blog",
                    Posts =
                    {
                        new Post { Title = "EF Core 3.1!" },
                        new Post { Title = "EF Core 5.0!" }
                    }
                });

            context.SaveChanges();
        }

        using (var context = new BlogsContext())
        {
            var blog = context.Blogs.Include(e => e.Posts).Single();

            blog.Name = "EF Core Blog";
            context.Remove(blog.Posts.First());
            blog.Posts.Add(new Post { Title = "EF Core 6.0!" });

            context.SaveChanges();
        }
        #endregion
    }
-->
[!code-csharp[Program](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=Program)]

<span data-ttu-id="7dcd5-133">此代码的输出显示检测到的事件：</span><span class="sxs-lookup"><span data-stu-id="7dcd5-133">The output from this code shows the events detected:</span></span>

```output
EF is initializing BlogsContext
EF is opening a connection to Data Source=blogs.db;Mode=ReadOnly
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to Data Source=blogs.db;Mode=ReadOnly
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to DataSource=blogs.db
EF is initializing BlogsContext
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to DataSource=blogs.db
```
