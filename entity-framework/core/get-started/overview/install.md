---
title: 安装 Entity Framework Core - EF Core
description: Entity Framework Core 的安装说明
author: bricelam
ms.date: 08/06/2017
uid: core/get-started/overview/install
ms.openlocfilehash: 039e5339a6dba57f6a8f33acba6e467f8d6439c9
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431162"
---
# <a name="installing-entity-framework-core"></a>安装 Entity Framework Core

## <a name="prerequisites"></a>先决条件

* EF Core 是一个 [.NET Standard 2.0](/dotnet/standard/net-standard) 库。 因此，EF Core 需要支持运行 .NET Standard 2.0 的实现。 其他 .NET Standard 2.0 库也可引用 EF Core。

* 例如，可使用 EF Core 开发面向 .NET Core 的应用。 生成 .NET Core 应用需要 [.NET Core SDK](https://dotnet.microsoft.com/download)。 还可选择使用 [Visual Studio](https://visualstudio.microsoft.com/vs)[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac) 或 [Visual Studio Code](https://code.visualstudio.com) 等开发环境。 有关详细信息，请参阅 [.NET Core 入门](/dotnet/core/get-started)。

* 可在 Windows 上的 Visual Studio 中使用 EF Core 来开发应用程序。 建议使用最新版本的 [Visual Studio](https://visualstudio.microsoft.com/vs)。

* EF Core 可以在 [Xamarin](https://dotnet.microsoft.com/apps/xamarin) 和 .NET Native 等其他 .NET 实现上运行。 但在实践中，这些实现具有运行时限制，可能会影响 EF Core 处理应用的效率。 有关详细信息，请参阅 [EF Core 支持的 .NET 实现](xref:core/miscellaneous/platforms)。

* 最后，不同的数据库提供程序可能需要特定的数据库引擎版本、.NET 实现或操作系统。 请确保可用的 [EF Core 数据库提供程序](xref:core/providers/index)支持适用于应用程序的环境。

## <a name="get-the-entity-framework-core-runtime"></a>获取 Entity Framework Core 运行时

要将 EF Core 添加到应用程序，请安装适用于要使用的数据库提供程序的 NuGet 包。

如果要生成 ASP.NET Core 应用程序，不需要安装内存中和 SQL Server 提供程序。 这些提供程序随 EF Core 运行时一起包含在当前版本的 ASP.NET Core 中。

要安装或更新 NuGet 包，可以使用 .NET Core 命令行界面 (CLI)、Visual Studio 包管理器对话框或 Visual Studio 包管理器控制台。

### <a name="net-core-cli"></a>.NET Core CLI

* 在操作系统的命令行中使用以下 .NET Core CLI 命令来安装或更新 EF Core SQL Server 提供程序：

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* 可以使用 `-v` 修饰符在 `dotnet add package` 命令中指明特定的版本。 例如，若要安装 EF Core 2.2.0 包，请将 `-v 2.2.0` 追加到命令中。

有关详细信息，请参阅 [.NET 命令行接口 (CLI) 工具](/dotnet/core/tools/)。

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet 包管理器对话框

* 从 Visual Studio 菜单中选择“项目”>“管理 NuGet 包”

* 单击“浏览”或“更新”选项卡

* 若要安装或更新 SQL Server 提供程序，请选择 `Microsoft.EntityFrameworkCore.SqlServer` 包并确认。

有关详细信息，请参阅 [NuGet 包管理器对话框](/nuget/tools/package-manager-ui)。

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio NuGet 包管理器控制台

* 从 Visual Studio 菜单中选择“工具”>“NuGet 包管理器”>“包管理器控制台”

* 若要安装 SQL Server 提供程序，请在包管理器控制台中运行以下命令：

  ```powershell
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* 若要更新提供程序，使用 `Update-Package` 命令。

* 若要指定特定版本，可以使用 `-Version` 修饰符。 例如，若要安装 EF Core 2.2.0 包，请将 `-Version 2.2.0` 追加到命令中

有关详细信息，请参阅[包管理器控制台](/nuget/tools/package-manager-console)。

## <a name="get-the-entity-framework-core-tools"></a>获取 Entity Framework Core 工具

可以安装工具来执行项目中与 EF Core 相关的任务，例如创建和应用数据库迁移，或基于现有数据库创建 EF Core 模型。

提供了两个工具集：

* [.NET Core 命令行接口 (CLI) 工具](xref:core/cli/dotnet)可用于 Windows、Linux 或 macOS。 这些命令以 `dotnet ef` 开头。

* [包管理器控制台 (PMC) 工具](xref:core/cli/powershell)在 Windows 上的 Visual Studio 中运行。 这些命令以动词开头，例如 `Add-Migration`、`Update-Database`。

虽然也可在包管理器控制台中使用 `dotnet ef` 命令，但在使用 Visual Studio 时建议使用包管理器控制台工具：

* 它们会自动使用在 Visual Studio 的 PMC 中选择的当前项目，无需手动切换目录。

* 命令完成后，它们会自动在 Visual Studio 中打开命令所生成的文件。

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>获取 .NET Core CLI 工具

.NET core CLI 工具需要前面的[系统必备](#prerequisites)中提到的 .NET Core SDK。

* 必须将 `dotnet ef` 安装为全局工具或本地工具。 大多数开发人员偏向于使用以下命令将 `dotnet ef` 安装为全局工具：

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  ```

  `dotnet ef` 还可用作本地工具。 若要将它用作本地工具，请使用[工具清单文件](/dotnet/core/tools/global-tools#install-a-local-tool)恢复声明为工具依赖项的项目的依赖项。

* 若要更新工具，请使用 `dotnet tool update` 命令。

* 安装最新的 `Microsoft.EntityFrameworkCore.Design` 包。

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

> [!IMPORTANT]
> 请务必使用与运行时包主版本匹配的工具包版本。

### <a name="get-the-package-manager-console-tools"></a>获取包管理器控制台工具

若要获取适用于 EF Core 的包管理器控制台工具，请安装 `Microsoft.EntityFrameworkCore.Tools` 包。 例如，在 Visual Studio 中：

```powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

ASP.NET Core 应用自动随附此包。

## <a name="upgrading-to-the-latest-ef-core"></a>升级到最新版本的 EF Core

* 每次发布新版本的 EF Core 时，我们都还会在 EF Core 项目中发布新版本的提供程序，如 Microsoft.EntityFrameworkCore.SqlServer、Microsoft.EntityFrameworkCore.Sqlite 和 Microsoft.EntityFrameworkCore.InMemory。 只需升级到新版本的提供程序即可获取所有改进。

* EF Core 以及 SQL Server 和内存中提供程序包含在当前版本的 ASP.NET Core 中。 若要将现有 ASP.NET Core 应用程序升级到较新版本的 EF Core，请务必升级 ASP.NET Core 的版本。

* 如需更新使用第三方数据库提供程序的应用程序，请始终检查与要使用的 EF Core 版本兼容的提供程序有无更新。 例如，版本 1.0 的数据库提供程序与版本 2.0 的 EF Core 运行时不兼容。

* 用于 EF Core 的第三方提供程序通常不随 EF Core 运行发布修补程序版本。 若要将使用第三方提供程序的应用程序升级到 EF Core 的修补程序版本，可能需要添加对单独的 EF Core 运行时组件（如 Microsoft.EntityFrameworkCore 和 Microsoft.EntityFrameworkCore.Relational）的直接引用。
