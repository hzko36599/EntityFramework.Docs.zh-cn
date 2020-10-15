---
title: Visual Studio 版本-EF6
description: Visual Studio 版本和实体框架6
author: ajcvickers
ms.date: 07/05/2018
uid: ef6/what-is-new/visual-studio
ms.openlocfilehash: ba6d8255f1cb5393b2f0e3e404f4fd2d1f1621c4
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064232"
---
# <a name="visual-studio-releases"></a><span data-ttu-id="7887f-103">Visual Studio 版本</span><span class="sxs-lookup"><span data-stu-id="7887f-103">Visual Studio Releases</span></span>

<span data-ttu-id="7887f-104">建议始终使用最新版本的 Visual Studio，因为它包含适用于 .NET、NuGet 和实体框架的最新工具。</span><span class="sxs-lookup"><span data-stu-id="7887f-104">We recommend to always use the latest version of Visual Studio because it contains the latest tools for .NET, NuGet, and Entity Framework.</span></span>
<span data-ttu-id="7887f-105">事实上，实体框架文档中的各种示例和演练都假设你使用的是最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="7887f-105">In fact, the various samples and walkthroughs across the Entity Framework documentation assume that you are using a recent version of Visual Studio.</span></span>

<span data-ttu-id="7887f-106">不过，只要考虑一些差异，就有可能使用不同版本的 Visual Studio 的较旧版本实体框架：</span><span class="sxs-lookup"><span data-stu-id="7887f-106">It is possible however to use older versions of Visual Studio with different versions of Entity Framework as long as you take into account some differences:</span></span>

## <a name="visual-studio-2017-157-and-newer"></a><span data-ttu-id="7887f-107">Visual Studio 2017 15.7 及更高版本</span><span class="sxs-lookup"><span data-stu-id="7887f-107">Visual Studio 2017 15.7 and newer</span></span>

- <span data-ttu-id="7887f-108">此版本的 Visual Studio 包括实体框架工具和 EF 6.2 运行时的最新版本，不需要执行其他设置步骤。</span><span class="sxs-lookup"><span data-stu-id="7887f-108">This version of Visual Studio includes the latest release of Entity Framework tools and the EF 6.2 runtime, and does not require additional setup steps.</span></span>
<span data-ttu-id="7887f-109">有关这些版本的详细信息，请参阅 [新增功能](xref:ef6/what-is-new/index) 。</span><span class="sxs-lookup"><span data-stu-id="7887f-109">See [What's New](xref:ef6/what-is-new/index) for more details on these releases.</span></span>
- <span data-ttu-id="7887f-110">使用 EF 工具将实体框架添加到新项目将自动添加 EF 6.2 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-110">Adding Entity Framework to new projects using the EF tools will automatically add the EF 6.2 NuGet package.</span></span>
<span data-ttu-id="7887f-111">可以手动安装或升级到联机提供的任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-111">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="7887f-112">默认情况下，此版本的 Visual Studio 中可用的 SQL Server 实例为 LocalDB 实例，名为 MSSQLLocalDB。</span><span class="sxs-lookup"><span data-stu-id="7887f-112">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="7887f-113">应使用的连接字符串的服务器部分是 " (localdb) \\ MSSQLLocalDB"。</span><span class="sxs-lookup"><span data-stu-id="7887f-113">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="7887f-114">`@` \\ \\ 在 c # 代码中指定连接字符串时，请记住使用前缀为或双反斜杠 "" 的逐字字符串。</span><span class="sxs-lookup"><span data-stu-id="7887f-114">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a><span data-ttu-id="7887f-115">Visual Studio 2015 到 Visual Studio 2017 15。6</span><span class="sxs-lookup"><span data-stu-id="7887f-115">Visual Studio 2015 to Visual Studio 2017 15.6</span></span>

- <span data-ttu-id="7887f-116">这些版本的 Visual Studio 包括实体框架工具和运行时 ef6.1.3。</span><span class="sxs-lookup"><span data-stu-id="7887f-116">These versions of Visual Studio include Entity Framework tools and runtime 6.1.3.</span></span>
<span data-ttu-id="7887f-117">有关这些版本的详细信息，请参阅 [以前的版本](xref:ef6/what-is-new/past-releases#ef-613) 。</span><span class="sxs-lookup"><span data-stu-id="7887f-117">See [Past Releases](xref:ef6/what-is-new/past-releases#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="7887f-118">使用 EF 工具向新项目添加实体框架会自动添加 EF ef6.1.3 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-118">Adding Entity Framework to new projects using the EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="7887f-119">可以手动安装或升级到联机提供的任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-119">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="7887f-120">默认情况下，此版本的 Visual Studio 中可用的 SQL Server 实例为 LocalDB 实例，名为 MSSQLLocalDB。</span><span class="sxs-lookup"><span data-stu-id="7887f-120">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="7887f-121">应使用的连接字符串的服务器部分是 " (localdb) \\ MSSQLLocalDB"。</span><span class="sxs-lookup"><span data-stu-id="7887f-121">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="7887f-122">`@` \\ \\ 在 c # 代码中指定连接字符串时，请记住使用前缀为或双反斜杠 "" 的逐字字符串。</span><span class="sxs-lookup"><span data-stu-id="7887f-122">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  


## <a name="visual-studio-2013"></a><span data-ttu-id="7887f-123">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7887f-123">Visual Studio 2013</span></span>
- <span data-ttu-id="7887f-124">此版本的 Visual Studio 包括实体框架工具和运行时的旧版本。</span><span class="sxs-lookup"><span data-stu-id="7887f-124">This version of Visual Studio includes and older version of Entity Framework tools and runtime.</span></span>
<span data-ttu-id="7887f-125">建议使用 Microsoft 下载中心中提供 [的安装程序](https://www.microsoft.com/download/details.aspx?id=40762) Entity Framework Tools ef6.1.3 升级到。</span><span class="sxs-lookup"><span data-stu-id="7887f-125">It is recommended that you upgrade to Entity Framework Tools 6.1.3, using [the installer](https://www.microsoft.com/download/details.aspx?id=40762) available in the Microsoft Download Center.</span></span>
<span data-ttu-id="7887f-126">有关这些版本的详细信息，请参阅 [以前的版本](xref:ef6/what-is-new/past-releases#ef-613) 。</span><span class="sxs-lookup"><span data-stu-id="7887f-126">See [Past Releases](xref:ef6/what-is-new/past-releases#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="7887f-127">使用升级后的 EF 工具向新项目添加实体框架会自动添加 EF ef6.1.3 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-127">Adding Entity Framework to new projects using the upgraded EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="7887f-128">可以手动安装或升级到联机提供的任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-128">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="7887f-129">默认情况下，此版本的 Visual Studio 中可用的 SQL Server 实例为 LocalDB 实例，名为 MSSQLLocalDB。</span><span class="sxs-lookup"><span data-stu-id="7887f-129">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="7887f-130">应使用的连接字符串的服务器部分是 " (localdb) \\ MSSQLLocalDB"。</span><span class="sxs-lookup"><span data-stu-id="7887f-130">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="7887f-131">`@` \\ \\ 在 c # 代码中指定连接字符串时，请记住使用前缀为或双反斜杠 "" 的逐字字符串。</span><span class="sxs-lookup"><span data-stu-id="7887f-131">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  

## <a name="visual-studio-2012"></a><span data-ttu-id="7887f-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7887f-132">Visual Studio 2012</span></span>

- <span data-ttu-id="7887f-133">此版本的 Visual Studio 包括实体框架工具和运行时的旧版本。</span><span class="sxs-lookup"><span data-stu-id="7887f-133">This version of Visual Studio includes and older version of Entity Framework tools and runtime.</span></span>
<span data-ttu-id="7887f-134">建议使用 Microsoft 下载中心中提供 [的安装程序](https://www.microsoft.com/download/details.aspx?id=40762) Entity Framework Tools ef6.1.3 升级到。</span><span class="sxs-lookup"><span data-stu-id="7887f-134">It is recommended that you upgrade to Entity Framework Tools 6.1.3, using [the installer](https://www.microsoft.com/download/details.aspx?id=40762) available in the Microsoft Download Center.</span></span>
<span data-ttu-id="7887f-135">有关这些版本的详细信息，请参阅 [以前的版本](xref:ef6/what-is-new/past-releases#ef-613) 。</span><span class="sxs-lookup"><span data-stu-id="7887f-135">See [Past Releases](xref:ef6/what-is-new/past-releases#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="7887f-136">使用升级后的 EF 工具向新项目添加实体框架会自动添加 EF ef6.1.3 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-136">Adding Entity Framework to new projects using the upgraded EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="7887f-137">可以手动安装或升级到联机提供的任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="7887f-137">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="7887f-138">默认情况下，此版本的 Visual Studio 中可用的 SQL Server 实例是名为 "v 11.0" 的 LocalDB 实例。</span><span class="sxs-lookup"><span data-stu-id="7887f-138">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called v11.0.</span></span>
<span data-ttu-id="7887f-139">应使用的连接字符串的服务器部分是 " (localdb) \\ v 11.0"。</span><span class="sxs-lookup"><span data-stu-id="7887f-139">The server section of connection string you should use is "(localdb)\\v11.0".</span></span>
<span data-ttu-id="7887f-140">`@` \\ \\ 在 c # 代码中指定连接字符串时，请记住使用前缀为或双反斜杠 "" 的逐字字符串。</span><span class="sxs-lookup"><span data-stu-id="7887f-140">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  

## <a name="visual-studio-2010"></a><span data-ttu-id="7887f-141">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="7887f-141">Visual Studio 2010</span></span>

- <span data-ttu-id="7887f-142">此版本的 Visual Studio 中提供的 Entity Framework Tools 版本与实体框架6运行时不兼容，因此无法升级。</span><span class="sxs-lookup"><span data-stu-id="7887f-142">The version of Entity Framework Tools available with this version of Visual Studio is not compatible with the Entity Framework 6 runtime and cannot be upgraded.</span></span>
- <span data-ttu-id="7887f-143">默认情况下，实体框架工具会将实体框架4.0 添加到你的项目。</span><span class="sxs-lookup"><span data-stu-id="7887f-143">By default, the Entity Framework tools will add Entity Framework 4.0 to your projects.</span></span>
<span data-ttu-id="7887f-144">若要使用任何更高版本的 EF 创建应用程序，首先需要安装 [NuGet 包管理器扩展](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager)。</span><span class="sxs-lookup"><span data-stu-id="7887f-144">In order to create applications using any newer versions of EF, you will first need to install the [NuGet Package Manager extension](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).</span></span>
- <span data-ttu-id="7887f-145">默认情况下，EF 工具版本中的所有代码生成都基于 EntityObject 和实体框架4。</span><span class="sxs-lookup"><span data-stu-id="7887f-145">By default, all code generation in the version of EF tools is based on EntityObject and Entity Framework 4.</span></span>
<span data-ttu-id="7887f-146">建议通过安装适用于 [c #](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) 或 [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET)的 DbContext 代码生成模板，将代码生成切换为基于 DbContext 和实体框架5。</span><span class="sxs-lookup"><span data-stu-id="7887f-146">We recommend that you switch the code generation to be based on DbContext and Entity Framework 5, by installing the DbContext code generation templates for [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) or [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).</span></span>
- <span data-ttu-id="7887f-147">安装 NuGet 包管理器扩展后，可以手动安装或升级到任何联机可用的 EF NuGet 包，并将 EF6 与 Code First 一起使用，这不需要设计器。</span><span class="sxs-lookup"><span data-stu-id="7887f-147">Once you have installed the NuGet Package Manager extensions, you can manually install or upgrade to any EF NuGet package available online and use EF6 with Code First, which does not require a designer.</span></span>
- <span data-ttu-id="7887f-148">默认情况下，此版本的 Visual Studio 中提供的 SQL Server 实例 SQL Server Express 称为 SQLEXPRESS。</span><span class="sxs-lookup"><span data-stu-id="7887f-148">By default, the SQL Server instance available with this version of Visual Studio is SQL Server Express named SQLEXPRESS.</span></span>
<span data-ttu-id="7887f-149">应使用的连接字符串的服务器部分是 ""。 \\SQLEXPRESS "。</span><span class="sxs-lookup"><span data-stu-id="7887f-149">The server section of connection string you should use is ".\\SQLEXPRESS".</span></span>
<span data-ttu-id="7887f-150">`@` \\ \\ 在 c # 代码中指定连接字符串时，请记住使用前缀为或双反斜杠 "" 的逐字字符串。</span><span class="sxs-lookup"><span data-stu-id="7887f-150">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>
