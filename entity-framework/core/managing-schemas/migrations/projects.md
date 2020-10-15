---
title: 使用单独的迁移项目-EF Core
description: 使用单独的迁移项目来管理具有 Entity Framework Core 的数据库架构
author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 6f28027c320f0d1c13d13bef7d8227b2bb68df91
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062485"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="ce029-103">使用单独的迁移项目</span><span class="sxs-lookup"><span data-stu-id="ce029-103">Using a Separate Migrations Project</span></span>

<span data-ttu-id="ce029-104">你可能想要将迁移存储在与包含你的不同的程序集中 `DbContext` 。</span><span class="sxs-lookup"><span data-stu-id="ce029-104">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="ce029-105">你还可以使用此策略来维护多个迁移集，例如，一个用于开发，另一个用于发布到发布升级。</span><span class="sxs-lookup"><span data-stu-id="ce029-105">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="ce029-106">若要执行此操作...</span><span class="sxs-lookup"><span data-stu-id="ce029-106">To do this...</span></span>

1. <span data-ttu-id="ce029-107">创建一个新的类库。</span><span class="sxs-lookup"><span data-stu-id="ce029-107">Create a new class library.</span></span>

2. <span data-ttu-id="ce029-108">添加对 DbContext 程序集的引用。</span><span class="sxs-lookup"><span data-stu-id="ce029-108">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="ce029-109">将迁移和模型快照文件移动到类库。</span><span class="sxs-lookup"><span data-stu-id="ce029-109">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="ce029-110">如果没有现有迁移，请在包含 DbContext 的项目中生成一个迁移，然后移动它。</span><span class="sxs-lookup"><span data-stu-id="ce029-110">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="ce029-111">这一点很重要，因为如果迁移程序集不包含现有迁移，Add-Migration 命令将无法找到 DbContext。</span><span class="sxs-lookup"><span data-stu-id="ce029-111">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="ce029-112">配置迁移程序集：</span><span class="sxs-lookup"><span data-stu-id="ce029-112">Configure the migrations assembly:</span></span>

   ```csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="ce029-113">从启动程序集添加对迁移程序集的引用。</span><span class="sxs-lookup"><span data-stu-id="ce029-113">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="ce029-114">如果这导致循环依赖项，请更新类库的输出路径：</span><span class="sxs-lookup"><span data-stu-id="ce029-114">If this causes a circular dependency, update the output path of the class library:</span></span>

     ```xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="ce029-115">如果一切正常，应能够向项目添加新的迁移。</span><span class="sxs-lookup"><span data-stu-id="ce029-115">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-cli"></a>[<span data-ttu-id="ce029-116">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ce029-116">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[<span data-ttu-id="ce029-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce029-117">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
