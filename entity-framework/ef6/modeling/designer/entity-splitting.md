---
title: 设计器实体拆分-EF6
description: 实体框架6中的设计器实体拆分
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/entity-splitting
ms.openlocfilehash: 8fdb9037fc49045089016e6630316d05c8726ad8
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064895"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="4d2ff-103">设计器实体拆分</span><span class="sxs-lookup"><span data-stu-id="4d2ff-103">Designer Entity Splitting</span></span>
<span data-ttu-id="4d2ff-104">本演练演示如何通过使用 Entity Framework Designer (EF 设计器) 修改模型，将实体类型映射到两个表。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-104">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="4d2ff-105">如果多个表共享同一个键，则可以将一个实体映射到这些表。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-105">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="4d2ff-106">适用于将一个实体类型映射到两个表的概念，可以轻松扩展为将一个实体类型映射到两个以上的表。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-106">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="4d2ff-107">下图显示了在使用 EF 设计器时使用的主窗口。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 设计器](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="4d2ff-109">先决条件</span><span class="sxs-lookup"><span data-stu-id="4d2ff-109">Prerequisites</span></span>

<span data-ttu-id="4d2ff-110">Visual Studio 2012 或 Visual Studio 2010、旗舰版、高级版、专业版或 Web Express edition。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-110">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="4d2ff-111">创建数据库</span><span class="sxs-lookup"><span data-stu-id="4d2ff-111">Create the Database</span></span>

<span data-ttu-id="4d2ff-112">随 Visual Studio 一起安装的数据库服务器因安装的 Visual Studio 版本而异：</span><span class="sxs-lookup"><span data-stu-id="4d2ff-112">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="4d2ff-113">如果使用的是 Visual Studio 2012，则将创建一个 LocalDB 数据库。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-113">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="4d2ff-114">如果使用的是 Visual Studio 2010，则将创建 SQL Express 数据库。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-114">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="4d2ff-115">首先，我们将创建一个包含两个表的数据库，我们会将这些表合并到一个实体中。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-115">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="4d2ff-116">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d2ff-116">Open Visual Studio</span></span>
-   <span data-ttu-id="4d2ff-117">**视图- &gt; 服务器资源管理器**</span><span class="sxs-lookup"><span data-stu-id="4d2ff-117">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="4d2ff-118">右键单击 "**数据连接- &gt; 添加连接 ...** "</span><span class="sxs-lookup"><span data-stu-id="4d2ff-118">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="4d2ff-119">如果尚未从服务器资源管理器连接到数据库，则需要选择 **Microsoft SQL Server** 作为数据源</span><span class="sxs-lookup"><span data-stu-id="4d2ff-119">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="4d2ff-120">连接到 LocalDB 或 SQL Express，具体取决于你安装的是哪个</span><span class="sxs-lookup"><span data-stu-id="4d2ff-120">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="4d2ff-121">输入 **EntitySplitting** 作为数据库名称</span><span class="sxs-lookup"><span data-stu-id="4d2ff-121">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="4d2ff-122">选择 **"确定"** ，系统会询问您是否要创建新数据库，请选择 **"是"**</span><span class="sxs-lookup"><span data-stu-id="4d2ff-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="4d2ff-123">新数据库现在将出现在服务器资源管理器</span><span class="sxs-lookup"><span data-stu-id="4d2ff-123">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="4d2ff-124">如果使用的是 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4d2ff-124">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="4d2ff-125">右键单击“服务器资源管理器”中的数据库，然后选择“新建查询”\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="4d2ff-125">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="4d2ff-126">将以下 SQL 复制到新的查询中，然后右键单击该查询，然后选择 "**执行**"</span><span class="sxs-lookup"><span data-stu-id="4d2ff-126">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="4d2ff-127">如果使用的是 Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="4d2ff-127">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="4d2ff-128">选择 **数据 &gt; Transact-sql SQL 编辑器- &gt; 新建查询连接 ...**</span><span class="sxs-lookup"><span data-stu-id="4d2ff-128">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="4d2ff-129">输入 **。 \\SQLEXPRESS** 作为服务器名称，然后单击 **"确定"**</span><span class="sxs-lookup"><span data-stu-id="4d2ff-129">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="4d2ff-130">从 "查询编辑器" 顶部的下拉菜单中选择 " **EntitySplitting** " 数据库</span><span class="sxs-lookup"><span data-stu-id="4d2ff-130">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="4d2ff-131">将以下 SQL 复制到新的查询中，然后右键单击该查询，然后选择 "**执行 SQL** "。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-131">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="4d2ff-132">创建项目</span><span class="sxs-lookup"><span data-stu-id="4d2ff-132">Create the Project</span></span>

-   <span data-ttu-id="4d2ff-133">在 **“文件”** 菜单上，指向 **“新建”** ，再单击 **“项目”** 。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-133">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="4d2ff-134">在左窗格中，单击 " **Visual \# C**"，然后选择 "**控制台应用程序**" 模板。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-134">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="4d2ff-135">输入 **MapEntityToTablesSample** 作为项目名称，然后单击 **"确定"**。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-135">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="4d2ff-136">如果系统提示保存在第一部分中创建的 SQL 查询，请单击 " **否** "。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-136">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="4d2ff-137">创建基于数据库的模型</span><span class="sxs-lookup"><span data-stu-id="4d2ff-137">Create a Model based on the Database</span></span>

-   <span data-ttu-id="4d2ff-138">右键单击 "解决方案资源管理器中的项目名称，指向" **添加**"，然后单击" **新建项**"。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-138">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="4d2ff-139">从左侧菜单中选择 " **数据** "，然后在 "模板" 窗格中选择 " **ADO.NET 实体数据模型** 。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-139">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="4d2ff-140">输入 **MapEntityToTablesModel** 作为文件名，然后单击 " **添加**"。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-140">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="4d2ff-141">在 "选择模型内容" 对话框中，选择 " **从数据库生成**"，然后单击 " **下一步"。**</span><span class="sxs-lookup"><span data-stu-id="4d2ff-141">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="4d2ff-142">从下拉菜单中选择 " **EntitySplitting** " 连接，然后单击 " **下一步**"。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-142">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="4d2ff-143">在 "选择数据库对象" 对话框中，选中 "**表**" 节点旁边的复选框   。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-143">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="4d2ff-144">这会将 **EntitySplitting** 数据库中的所有表添加到模型。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-144">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="4d2ff-145">单击 " **完成**"。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-145">Click **Finish**.</span></span>

<span data-ttu-id="4d2ff-146">此时会显示 Entity Designer，它提供了用于编辑模型的设计图面。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-146">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="4d2ff-147">将一个实体映射到两个表</span><span class="sxs-lookup"><span data-stu-id="4d2ff-147">Map an Entity to Two Tables</span></span>

<span data-ttu-id="4d2ff-148">在此步骤中，我们将更新 **person** 实体类型以合并 **person** 和 **PersonInfo** 表中的数据。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-148">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="4d2ff-149">选择 PersonInfo 实体的 **电子邮件**   和**电话**属性，并按**Ctrl + X**键。 **PersonInfo **</span><span class="sxs-lookup"><span data-stu-id="4d2ff-149">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="4d2ff-150">选择 " **人员 **" 实体，并按 **Ctrl + V** 键。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-150">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="4d2ff-151">在设计图面上，选择 " **PersonInfo**"   实体，并按键盘上的 "**删除**" 按钮。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-151">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="4d2ff-152">当系统询问你是否要从模型中删除**PersonInfo**表时，请单击 "**否**"，我们将把它映射到**Person**实体。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-152">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![删除表](~/ef6/media/deletetables.png)

<span data-ttu-id="4d2ff-154">接下来的步骤需要 " **映射详细信息**"   窗口。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-154">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="4d2ff-155">如果看不到此窗口，请右键单击设计图面，然后选择 " **映射详细信息**"。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-155">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="4d2ff-156">选择 " **人员**"   实体类型，然后在 "映射详细信息" 窗口中单击 " \*\* &lt; 添加表或视图 &gt; \*\*   " **Mapping Details**   。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-156">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="4d2ff-157"> **PersonInfo **   从下拉列表中选择 "PersonInfo"。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-157">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="4d2ff-158">" **映射详细信息**"   窗口将用默认的列映射进行更新，它们适用于我们的方案。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-158">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="4d2ff-159"> **Person**   实体类型现在映射到 **person**   和 **PersonInfo**   表。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-159">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![映射2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="4d2ff-161">使用模型</span><span class="sxs-lookup"><span data-stu-id="4d2ff-161">Use the Model</span></span>

-   <span data-ttu-id="4d2ff-162">将以下代码粘贴到 Main 方法中。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-162">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="4d2ff-163">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-163">Compile and run the application.</span></span>

<span data-ttu-id="4d2ff-164">由于运行此应用程序，对数据库执行了以下 T-sql 语句。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-164">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="4d2ff-165">以下两个 **INSERT** 语句是执行上下文的结果。SaveChanges ( # A1。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-165">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="4d2ff-166">它们采用 **person** 实体的数据，并将其拆分为 **person** 和 **PersonInfo** 表。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-166">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![插入1](~/ef6/media/insert1.png)

    ![插入2](~/ef6/media/insert2.png)
-   <span data-ttu-id="4d2ff-169">由于枚举数据库中的人员，因此执行了下面的 **SELECT** 。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-169">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="4d2ff-170">它将 **Person** 和 **PersonInfo** 表中的数据合并在一起。</span><span class="sxs-lookup"><span data-stu-id="4d2ff-170">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![选择合并人员和 PersonInfo 数据](~/ef6/media/select.png)
