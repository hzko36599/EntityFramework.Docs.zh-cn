---
title: 新增功能 - EF6
description: Entity Framework 6 中的新增功能
author: ajcvickers
ms.date: 09/12/2019
uid: ef6/what-is-new/index
ms.openlocfilehash: 3c588475268ea063433f55fdb2f994d70f8c51ae
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064297"
---
# <a name="whats-new-in-ef6"></a>EF6 中的新增功能

强烈建议使用最新发布的实体框架版本，以确保获得最新功能和最高稳定性。
但我们也意识到用户可能需要使用以前的版本，或想要尝试最新预发行版中的新改进。
若要安装特定版本的 EF，请参阅[获取实体框架](xref:ef6/fundamentals/install)。

## <a name="ef-640"></a>EF 6.4.0

EF 6.4.0 运行时已于 2019 年 12 月发布到 NuGet。 EF 6.4 的主要目标是优化 EF 6.3 中提供的功能和方案。 请参阅 Github 上的[重要修复列表](https://github.com/dotnet/ef6/milestone/14?closed=1)。

## <a name="ef-630"></a>EF 6.3.0

EF 6.3.0 运行时已于 2019 年 9 月发布到 NuGet。 此版本的主要目标是帮助将使用 EF 6 的现有应用程序迁移到 .NET Core 3.0。 社区还提供了多个 bug 修复和增强功能。 有关详细信息，请参阅每个 6.3.0 [里程碑](https://github.com/aspnet/EntityFramework6/milestones?state=closed)中关闭的问题。 下面是一些更值得注意的事项：

- 支持 .NET Core 3.0
  - 除了 .NET Framework 4.x，EntityFramework 包现在还面向 .NET Standard 2.1。
  - 这意味着 EF 6.3 是跨平台的，并在 Windows 之外的其他操作系统（如 Linux 和 macOS）上受支持。
  - 已重新编写迁移命令以在进程外执行，并使用 SDK 样式的项目。
- 支持 SQL Server HierarchyId。
- 提高了与 Roslyn 和 NuGet PackageReference 的兼容性。
- 添加了 `ef6.exe` 实体工具，用于启用、添加、编写脚本和应用程序集中的迁移。 这会替换 `migrate.exe`。

### <a name="ef-designer-support"></a>EF 设计器支持

目前不支持直接在 .NET Core 或 .NET Standard 项目上或在 SDK 样式的 .NET Framework 项目上使用 EF 设计器。 

可以通过在同一解决方案中将 EDMX 文件以及为实体和 DbContext 生成的类作为链接文件添加到 .NET Core 3.0 或 .NET Standard 2.1 项目中来解决此限制。

在项目文件中，链接文件将如下所示：

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

请注意，EDMX 文件与 EntityDeploy 生成操作关联。 这是一个特殊的 MSBuild 任务（现已包含在 EF 6.3 包中），它负责将 EF 模型作为嵌入式资源添加到目标程序集中（或将其复制为输出文件夹中的文件，具体取决于 EDMX 中的“元数据项目处理”设置）。 有关如何进行此设置的详细信息，请参阅 [EDMX .NET Core 示例](https://aka.ms/EdmxDotNetCoreSample)。

警告：请确保定义“实际”.edmx 文件的旧样式（即非 SDK 样式）.NET Framework 项目在定义 .sln 文件内的链接的项目之前。 否则，当在设计器中打开 .edmx 文件时，将看到错误消息“该实体框架在当前为项目指定的目标框架中不可用。 可以更改项目的目标框架或在 XmlEditor 中编辑模型”。

## <a name="past-releases"></a>过去的版本

[过去的版本](xref:ef6/what-is-new/past-releases)页包含过去所有版本的 EF 的存档以及各版本中引入的主要功能。
