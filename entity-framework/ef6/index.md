---
title: Entity Framework 6 概述 - EF6
description: Entity Framework 6 概述
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/index
ms.openlocfilehash: 09b7cb9e0141e9739b15d6c9789286c2e47018cc
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065389"
---
# <a name="entity-framework-6"></a><span data-ttu-id="8fc0a-103">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8fc0a-103">Entity Framework 6</span></span>
<span data-ttu-id="8fc0a-104">实体框架 6 (EF6) 是经试验和测试的关系映射器 (O/RM)，适用于 .NET 的对象，其功能和稳定性经过了多年的开发和调试。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-104">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="8fc0a-105">作为 O/RM，EF6 降低了关系方面和面向对象的方面之间的阻抗不匹配，使开发人员能够使用表示应用程序域的强类型 .NET 对象来编写应用程序，该应用程序可与存储在关系数据库中的数据交互，同时使开发人员无需再编写大部分的数据访问“管道”代码。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-105">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="8fc0a-106">EF6 可实现许多热门 O/RM 功能：</span><span class="sxs-lookup"><span data-stu-id="8fc0a-106">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="8fc0a-107">不依赖于任何 EF 类型的 [POCO](xref:ef6/resources/glossary#poco) 实体类的映射</span><span class="sxs-lookup"><span data-stu-id="8fc0a-107">Mapping of [POCO](xref:ef6/resources/glossary#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="8fc0a-108">自动更改跟踪</span><span class="sxs-lookup"><span data-stu-id="8fc0a-108">Automatic change tracking</span></span>
- <span data-ttu-id="8fc0a-109">标识解析和工作单元</span><span class="sxs-lookup"><span data-stu-id="8fc0a-109">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="8fc0a-110">预先、延迟和显式加载</span><span class="sxs-lookup"><span data-stu-id="8fc0a-110">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="8fc0a-111">使用 [LINQ（语言集成查询）](https://aka.ms/AA6hsvu)转换强类型查询</span><span class="sxs-lookup"><span data-stu-id="8fc0a-111">Translation of strongly-typed queries using [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span></span>
- <span data-ttu-id="8fc0a-112">丰富的映射功能，可支持：</span><span class="sxs-lookup"><span data-stu-id="8fc0a-112">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="8fc0a-113">一对一、一对多和多对多关系</span><span class="sxs-lookup"><span data-stu-id="8fc0a-113">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="8fc0a-114">继承（每个层次结构一张表、每个类型一张表和每个具体类一张表）</span><span class="sxs-lookup"><span data-stu-id="8fc0a-114">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="8fc0a-115">复杂类型</span><span class="sxs-lookup"><span data-stu-id="8fc0a-115">Complex types</span></span>
  - <span data-ttu-id="8fc0a-116">存储过程</span><span class="sxs-lookup"><span data-stu-id="8fc0a-116">Stored procedures</span></span>
- <span data-ttu-id="8fc0a-117">通过可视化设计器创建实体模型。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-117">A visual designer to create entity models.</span></span>
- <span data-ttu-id="8fc0a-118">通过编写代码创建实体模型的“Code First”体验。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-118">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="8fc0a-119">既可从现有数据库生成模型，然后手动编辑，也可从头开始创建模型，然后用于生成新的数据库。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-119">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="8fc0a-120">与 .NET Framework 应用程序模型（包括 ASP.NET）集成，并通过数据绑定与 WPF 和 WinForms 集成。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-120">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="8fc0a-121">基于 ADO.NET 的数据库连接和可用于连接到 SQL Server、Oracle、MySQL、SQLite、PostgreSQL、DB2 等的众多[提供程序](xref:ef6/fundamentals/providers/index)。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-121">Database connectivity based on ADO.NET and numerous [providers](xref:ef6/fundamentals/providers/index) available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="8fc0a-122">应使用 EF6 还是 EF Core？</span><span class="sxs-lookup"><span data-stu-id="8fc0a-122">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="8fc0a-123">EF Core 是更现代、可扩展的轻量级实体框架版本，与 EF6 的功能和优点非常相似。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-123">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="8fc0a-124">EF Core 则完全进行了重写，包含许多 EF6 没有的新功能，但还是缺少 EF6 中最高级的一些映射功能。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-124">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="8fc0a-125">如果功能集与需求匹配，请考虑在新应用程序中使用 EF Core。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-125">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="8fc0a-126">[比较 EF Core 和 EF6](xref:efcore-and-ef6/index)中更详细地讨论了此选项。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-126">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-started"></a>[<span data-ttu-id="8fc0a-127">入门</span><span class="sxs-lookup"><span data-stu-id="8fc0a-127">Get Started</span></span>](xref:ef6/get-started)

<span data-ttu-id="8fc0a-128">将 EntityFramework NuGet 包添加到项目或安装[适用于 Visual Studio 的 Entity Framework Tools](https://aka.ms/AA6i8c5)。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-128">Add the EntityFramework NuGet package to your project or install the [Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5).</span></span> <span data-ttu-id="8fc0a-129">然后观看视频、阅读教程和高级文档，以充分利用 EF6。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-129">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="8fc0a-130">过去的实体框架版本</span><span class="sxs-lookup"><span data-stu-id="8fc0a-130">Past Entity Framework Versions</span></span>

<span data-ttu-id="8fc0a-131">本文档针对的是最新版本的实体框架 6 ，但其中大部分内容也适用于过去的版本。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-131">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="8fc0a-132">请查看[新增功能](xref:ef6/what-is-new/index)和[过去的版本](xref:ef6/what-is-new/past-releases)，了解 EF 版本和其中引入的功能的完整列表。</span><span class="sxs-lookup"><span data-stu-id="8fc0a-132">Check out [What's New](xref:ef6/what-is-new/index) and [Past Releases](xref:ef6/what-is-new/past-releases) for a complete list of EF releases and the features they introduced.</span></span>
