---
title: 用 SQLite 测试 EF Core
description: 使用 SQLite 测试 Entity Framework Core 应用程序
author: ajcvickers
ms.date: 04/24/2020
uid: core/testing/sqlite
ms.openlocfilehash: ebfcd36bf236cb83cab8683a8c31d4752d437998
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431238"
---
# <a name="using-sqlite-to-test-an-ef-core-application"></a><span data-ttu-id="a24ea-103">使用 SQLite 测试 EF Core 应用程序</span><span class="sxs-lookup"><span data-stu-id="a24ea-103">Using SQLite to test an EF Core application</span></span>

> [!WARNING]
> <span data-ttu-id="a24ea-104">使用 SQLite 可以有效地测试 EF Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a24ea-104">Using SQLite can be an effective way to test an EF Core application.</span></span>
> <span data-ttu-id="a24ea-105">但是，当 SQLite 的行为与其他数据库系统不同时，可能会出现问题。</span><span class="sxs-lookup"><span data-stu-id="a24ea-105">However, problems can arise where SQLite behaves differently from other database systems.</span></span>
> <span data-ttu-id="a24ea-106">有关问题和利弊权衡的讨论，请参阅 [使用 EF Core 的测试代码](xref:core/testing/index) 。</span><span class="sxs-lookup"><span data-stu-id="a24ea-106">See [Testing code that uses EF Core](xref:core/testing/index) for a discussion of the issues and trade-offs.</span></span>  

<span data-ttu-id="a24ea-107">本文档在示例中所述的概念上生成了 [如何测试使用 EF Core 的应用程序](xref:core/testing/testing-sample)。</span><span class="sxs-lookup"><span data-stu-id="a24ea-107">This document builds uses on the concepts introduced in [Sample showing how to test applications that use EF Core](xref:core/testing/testing-sample).</span></span>
<span data-ttu-id="a24ea-108">此处所示的代码示例来自于此示例。</span><span class="sxs-lookup"><span data-stu-id="a24ea-108">The code examples shown here come from this sample.</span></span>

## <a name="using-sqlite-in-memory-databases"></a><span data-ttu-id="a24ea-109">使用 SQLite 内存中数据库</span><span class="sxs-lookup"><span data-stu-id="a24ea-109">Using SQLite in-memory databases</span></span>

<span data-ttu-id="a24ea-110">通常情况下，SQLite 以简单文件的形式创建数据库，并在应用程序中访问文件。</span><span class="sxs-lookup"><span data-stu-id="a24ea-110">Normally, SQLite creates databases as simple files and accesses the file in-process with your application.</span></span>
<span data-ttu-id="a24ea-111">这种速度非常快，尤其是在使用快速 [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)时。</span><span class="sxs-lookup"><span data-stu-id="a24ea-111">This is very fast, especially when using a fast [SSD](https://en.wikipedia.org/wiki/Solid-state_drive).</span></span>

<span data-ttu-id="a24ea-112">SQLite 还可以使用纯粹在内存中创建的数据库。</span><span class="sxs-lookup"><span data-stu-id="a24ea-112">SQLite can also use databases created purely in-memory.</span></span>
<span data-ttu-id="a24ea-113">只要了解内存中数据库生存期，就可以轻松地在 EF Core 中使用：</span><span class="sxs-lookup"><span data-stu-id="a24ea-113">This is easy to use with EF Core as long as you understand the in-memory database lifetime:</span></span>

* <span data-ttu-id="a24ea-114">当打开数据库的连接时，将创建该数据库</span><span class="sxs-lookup"><span data-stu-id="a24ea-114">The database is created when the connection to it is opened</span></span>
* <span data-ttu-id="a24ea-115">当数据库的连接关闭时，将删除该数据库</span><span class="sxs-lookup"><span data-stu-id="a24ea-115">The database is deleted when the connection to it is closed</span></span>

<span data-ttu-id="a24ea-116">EF Core 将在给定连接时使用已打开的连接，并且不会尝试关闭该连接。</span><span class="sxs-lookup"><span data-stu-id="a24ea-116">EF Core will use an already open connection when given one, and will never attempt to close it.</span></span>
<span data-ttu-id="a24ea-117">因此，将 EF Core 用于内存中 SQLite 数据库的关键是在将连接传递给 EF 之前打开连接。</span><span class="sxs-lookup"><span data-stu-id="a24ea-117">So the key to using EF Core with an in-memory SQLite database is to open the connection before passing it to EF.</span></span>  

<span data-ttu-id="a24ea-118">该 [示例](xref:core/testing/testing-sample) 通过以下代码实现此操作：</span><span class="sxs-lookup"><span data-stu-id="a24ea-118">The [sample](xref:core/testing/testing-sample) achieves this with the following code:</span></span>

[!code-csharp[SqliteInMemory](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/SqliteInMemoryItemsControllerTest.cs?name=SqliteInMemory)]

<span data-ttu-id="a24ea-119">注意：</span><span class="sxs-lookup"><span data-stu-id="a24ea-119">Notice:</span></span>

* <span data-ttu-id="a24ea-120">`CreateInMemoryDatabase`方法创建一个 SQLite 内存中数据库，并打开与该数据库的连接。</span><span class="sxs-lookup"><span data-stu-id="a24ea-120">The `CreateInMemoryDatabase` method creates a SQLite in-memory database and opens the connection to it.</span></span>
* <span data-ttu-id="a24ea-121">创建的 `DbConnection` 从中提取 `ContextOptions` ，并保存。</span><span class="sxs-lookup"><span data-stu-id="a24ea-121">The created `DbConnection` is extracted from the `ContextOptions` and saved.</span></span>
* <span data-ttu-id="a24ea-122">当释放测试以便不泄漏资源时，将释放该连接。</span><span class="sxs-lookup"><span data-stu-id="a24ea-122">The connection is disposed when the test is disposed so that resources are not leaked.</span></span>

> [!NOTE]
> <span data-ttu-id="a24ea-123">[问题 #16103](https://github.com/dotnet/efcore/issues/16103) 正在跟踪使此连接管理更容易的方法。</span><span class="sxs-lookup"><span data-stu-id="a24ea-123">[Issue #16103](https://github.com/dotnet/efcore/issues/16103) is tracking ways to make this connection management easier.</span></span>
