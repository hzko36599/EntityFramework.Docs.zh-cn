---
title: Table-Valued 函数 (Tvf) -EF6
description: 'Table-Valued 函数 (实体框架6中的 Tvf) '
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/tvfs
ms.openlocfilehash: 2c699cad3ace253842d17aca4eca4df2318c4795
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066195"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="05322-103">Table-Valued 函数 (Tvf) </span><span class="sxs-lookup"><span data-stu-id="05322-103">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="05322-104">**EF5 仅向前** -实体框架5中引入了本页中所述的功能、api 等。</span><span class="sxs-lookup"><span data-stu-id="05322-104">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="05322-105">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="05322-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="05322-106">视频和分步演练演示了如何使用 Entity Framework Designer 将表值函数映射 (Tvf) 。</span><span class="sxs-lookup"><span data-stu-id="05322-106">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="05322-107">它还演示了如何从 LINQ 查询调用 TVF。</span><span class="sxs-lookup"><span data-stu-id="05322-107">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="05322-108">Tvf 当前仅在 Database First 工作流中受支持。</span><span class="sxs-lookup"><span data-stu-id="05322-108">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="05322-109">实体框架版本5中引入了 TVF 支持。</span><span class="sxs-lookup"><span data-stu-id="05322-109">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="05322-110">请注意，若要使用表值函数、枚举和空间类型等新功能，则必须以 .NET Framework 4.5 为目标。</span><span class="sxs-lookup"><span data-stu-id="05322-110">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="05322-111">默认情况下，Visual Studio 2012 面向 .NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="05322-111">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="05322-112">Tvf 非常类似于具有一个关键区别的存储过程： TVF 的结果是可组合的。</span><span class="sxs-lookup"><span data-stu-id="05322-112">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="05322-113">这意味着，可以在 LINQ 查询中使用 TVF 的结果，而存储过程的结果不能。</span><span class="sxs-lookup"><span data-stu-id="05322-113">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="05322-114">观看视频</span><span class="sxs-lookup"><span data-stu-id="05322-114">Watch the video</span></span>

<span data-ttu-id="05322-115">**提供者**： Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="05322-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="05322-116">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv)  | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v)有  | [WMV (ZIP) ](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="05322-116">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="05322-117">先决条件</span><span class="sxs-lookup"><span data-stu-id="05322-117">Pre-Requisites</span></span>

<span data-ttu-id="05322-118">若要完成本演练，你需要：</span><span class="sxs-lookup"><span data-stu-id="05322-118">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="05322-119">安装 [School 数据库](xref:ef6/resources/school-database)。</span><span class="sxs-lookup"><span data-stu-id="05322-119">Install the [School database](xref:ef6/resources/school-database).</span></span>

- <span data-ttu-id="05322-120">具有最新版本的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="05322-120">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="05322-121">设置项目</span><span class="sxs-lookup"><span data-stu-id="05322-121">Set up the Project</span></span>

1.  <span data-ttu-id="05322-122">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="05322-122">Open Visual Studio</span></span>
2.  <span data-ttu-id="05322-123">在 "**文件**" 菜单上，指向 "**新建**"，然后单击 "**项目**"</span><span class="sxs-lookup"><span data-stu-id="05322-123">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="05322-124">在左窗格中，单击 " **Visual \# C**"，然后选择**控制台**模板</span><span class="sxs-lookup"><span data-stu-id="05322-124">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="05322-125">输入 **TVF** 作为项目名称，然后单击 **"确定"**</span><span class="sxs-lookup"><span data-stu-id="05322-125">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="05322-126">将 TVF 添加到数据库</span><span class="sxs-lookup"><span data-stu-id="05322-126">Add a TVF to the Database</span></span>

-   <span data-ttu-id="05322-127">选择 " **查看- &gt; SQL Server 对象资源管理器**</span><span class="sxs-lookup"><span data-stu-id="05322-127">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="05322-128">如果 LocalDB 不在服务器列表中：右键单击 " **SQL Server** "，然后选择 " **添加 SQL Server** 使用默认 **Windows 身份验证** 连接到 LocalDB 服务器</span><span class="sxs-lookup"><span data-stu-id="05322-128">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="05322-129">展开 LocalDB 节点</span><span class="sxs-lookup"><span data-stu-id="05322-129">Expand the LocalDB node</span></span>
-   <span data-ttu-id="05322-130">在 "数据库" 节点下，右键单击 "School" 数据库节点，然后选择 " **新建查询 ...** "</span><span class="sxs-lookup"><span data-stu-id="05322-130">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="05322-131">在 T-sql 编辑器中粘贴以下 TVF 定义</span><span class="sxs-lookup"><span data-stu-id="05322-131">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="05322-132">在 T-sql 编辑器上单击鼠标右键按钮，然后选择 " **执行**"</span><span class="sxs-lookup"><span data-stu-id="05322-132">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="05322-133">GetStudentGradesForCourse 函数将添加到 School 数据库</span><span class="sxs-lookup"><span data-stu-id="05322-133">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="05322-134">创建模型</span><span class="sxs-lookup"><span data-stu-id="05322-134">Create a Model</span></span>

1.  <span data-ttu-id="05322-135">右键单击 "解决方案资源管理器中的项目名称，指向"**添加**"，然后单击"**新建项**"</span><span class="sxs-lookup"><span data-stu-id="05322-135">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="05322-136">从左侧菜单中选择 "**数据**"，然后在 "**模板**" 窗格中选择 " **ADO.NET 实体数据模型**</span><span class="sxs-lookup"><span data-stu-id="05322-136">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="05322-137">输入**TVFModel**作为文件名，然后单击 "**添加**"</span><span class="sxs-lookup"><span data-stu-id="05322-137">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="05322-138">在 "选择模型内容" 对话框中，选择 " **从数据库生成**"，然后单击 " **下一步**"</span><span class="sxs-lookup"><span data-stu-id="05322-138">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="05322-139">单击 " **新建连接**"，在 "服务器名称" 文本框中\*\* (Localdb) \\ mssqllocaldb**输入**School\*\*   作为数据库名称单击 **"确定"**</span><span class="sxs-lookup"><span data-stu-id="05322-139">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="05322-140">在 "选择数据库对象" 对话框中的 " **表**" 节点下，   选择 " **Person**"、" **StudentGrade**" 和 " **课程**"   表</span><span class="sxs-lookup"><span data-stu-id="05322-140">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="05322-141">从 Visual Studio 2012 开始，选择 " **存储过程" 和 "函数**" 节点下的 **GetStudentGradesForCourse**函数，该   Entity Designer 允许批处理导入存储过程和函数</span><span class="sxs-lookup"><span data-stu-id="05322-141">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="05322-142">单击 " **完成**"</span><span class="sxs-lookup"><span data-stu-id="05322-142">Click **Finish**</span></span>
9.  <span data-ttu-id="05322-143">此时会显示 Entity Designer，它提供了用于编辑模型的设计图面。</span><span class="sxs-lookup"><span data-stu-id="05322-143">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="05322-144">您在 " **选择数据库对象**" 对话框中选择的所有对象   都将添加到模型中。</span><span class="sxs-lookup"><span data-stu-id="05322-144">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="05322-145">默认情况下，每个导入的存储过程或函数的结果形状将自动成为实体模型中的新复杂类型。</span><span class="sxs-lookup"><span data-stu-id="05322-145">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="05322-146">但我们想要将 GetStudentGradesForCourse 函数的结果映射到 StudentGrade 实体：右键单击设计图面并在模型浏览器中选择 " **模型浏览器**"，选择 " **函数导**入"，然后在 "编辑函数导入" 对话框中双击 **GetStudentGradesForCourse**函数，选择 " **实体**"，   然后选择 " **StudentGrade** "。</span><span class="sxs-lookup"><span data-stu-id="05322-146">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="05322-147">保留和检索数据</span><span class="sxs-lookup"><span data-stu-id="05322-147">Persist and Retrieve Data</span></span>

<span data-ttu-id="05322-148">打开定义 Main 方法的文件。</span><span class="sxs-lookup"><span data-stu-id="05322-148">Open the file where the Main method is defined.</span></span> <span data-ttu-id="05322-149">将以下代码添加到 Main 函数中。</span><span class="sxs-lookup"><span data-stu-id="05322-149">Add the following code into the Main function.</span></span>

<span data-ttu-id="05322-150">下面的代码演示如何生成使用表值函数的查询。</span><span class="sxs-lookup"><span data-stu-id="05322-150">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="05322-151">该查询将结果投影到一个匿名类型中，该类型包含相关课程标题和一个等级大于或等于3.5 的相关学生。</span><span class="sxs-lookup"><span data-stu-id="05322-151">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="05322-152">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="05322-152">Compile and run the application.</span></span> <span data-ttu-id="05322-153">该程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="05322-153">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="05322-154">摘要</span><span class="sxs-lookup"><span data-stu-id="05322-154">Summary</span></span>

<span data-ttu-id="05322-155">在本演练中，我们介绍了如何使用 Entity Framework Designer 将表值函数映射 (Tvf) 。</span><span class="sxs-lookup"><span data-stu-id="05322-155">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="05322-156">它还演示了如何从 LINQ 查询调用 TVF。</span><span class="sxs-lookup"><span data-stu-id="05322-156">It also demonstrated how to call a TVF from a LINQ query.</span></span>
