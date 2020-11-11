---
title: SQLite 数据库提供程序 - EF Core
description: 有关 Entity Framework Core SQLite 数据库提供程序的信息
author: bricelam
ms.date: 10/27/2016
uid: core/providers/sqlite/index
ms.openlocfilehash: 6dac555456ed347b3eb07bbd0f2a89ab9179d20e
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430295"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="73042-103">SQLite EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="73042-103">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="73042-104">此数据库提供程序允许将 Entity Framework Core 与 SQLite 一起使用。</span><span class="sxs-lookup"><span data-stu-id="73042-104">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="73042-105">该提供程序作为 [Entity Framework Core 项目](https://github.com/dotnet/efcore)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="73042-105">The provider is maintained as part of the [Entity Framework Core project](https://github.com/dotnet/efcore).</span></span>

## <a name="install"></a><span data-ttu-id="73042-106">安装</span><span class="sxs-lookup"><span data-stu-id="73042-106">Install</span></span>

<span data-ttu-id="73042-107">安装 [Microsoft.EntityFrameworkCore.Sqlite NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)。</span><span class="sxs-lookup"><span data-stu-id="73042-107">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="73042-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="73042-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="73042-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73042-109">Visual Studio</span></span>](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="73042-110">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="73042-110">Supported Database Engines</span></span>

* <span data-ttu-id="73042-111">SQLite（3.7 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="73042-111">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="73042-112">限制</span><span class="sxs-lookup"><span data-stu-id="73042-112">Limitations</span></span>

<span data-ttu-id="73042-113">有关 SQLite 提供程序的一些重要限制，请参阅 [SQLite 限制](xref:core/providers/sqlite/limitations)。</span><span class="sxs-lookup"><span data-stu-id="73042-113">See [SQLite Limitations](xref:core/providers/sqlite/limitations) for some important limitations of the SQLite provider.</span></span>
