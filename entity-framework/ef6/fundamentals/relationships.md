---
title: 关系、导航属性和外键-EF6
description: 实体框架6中的关系、导航属性和外键
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/relationships
ms.openlocfilehash: 956a24051c89d410e052ab02f073e693e1934a74
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062966"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>关系、导航属性和外键

本文概述了实体框架如何管理实体之间的关系。 它还提供了一些有关如何映射和操作关系的指导。

## <a name="relationships-in-ef"></a>EF 中的关系

在关系数据库中，关系 (也称为关联) 在表之间通过外键定义。 外键 (FK) 是用于建立和加强两个表数据之间的链接的一列或多列。 通常有三种类型的关系：一对一、一对多和多对多的关系。 在一对多关系中，外键在表示关系的多个端的表上定义。 多对多关系涉及到定义第三个表 (称为联接表或联接表) ，其主键由两个相关表中的外键构成。 在一对一关系中，主键除了作为外键以外，对于任何一个表都没有单独的外键列。

下图显示了参与一对多关系的两个表。 **课程**表是依赖表，因为它包含将其链接到**部门**表的**DepartmentID**列。

![部门和课程表](~/ef6/media/database2.png)

在实体框架中，可以通过关联或关系将实体与其他实体相关。 每个关系都包含两个端，分别描述了该关系中的两个实体 (一个、零或一或多个) 类型的实体类型和重数。 关系可由引用约束控制，该引用约束描述了关系中的哪端为 Principal Role 以及哪端为 Dependent Role。

导航属性提供了一种在两个实体类型之间导航关联的方法。 针对对象参与到其中的每个关系，各对象均可以具有导航属性。 使用导航属性，您可以在两个方向上导航和管理关系，如果重数为1或零或一个) ，则返回引用对象 (; 如果重数为多) ，则返回 (集合。 您还可以选择使用单向导航，在这种情况下，只需在参与关系的一种类型上定义导航属性，而不是在两者上定义。

建议在模型中包含映射到数据库中的外键的属性。 加入了外键属性，您就可以通过修改依赖对象的外键值来创建或更改关系。 此类关联称为外键关联。 在处理断开连接的实体时，使用外键更为重要。 请注意，当使用1对1或1到0时，请注意。1关系，没有单独的外键列，主键属性充当外键，并且始终包含在模型中。

如果模型中不包含外键列，则会将关联信息作为独立对象进行管理。 关系通过对象引用而不是外键属性进行跟踪。 这种类型的关联称为 *独立关联*。 修改 *独立关联* 的最常见方法是修改为参与关联的每个实体生成的导航属性。

可以在您的模型中选择使用一种或两种类型的关联。 但是，如果您具有通过仅包含外键的联接表连接的一种纯多对多关系，则 EF 将使用独立的关联来管理这类多对多关系。   

下图显示了使用 Entity Framework Designer 创建的概念模型。 该模型包含两个参与一对多关系的实体。 这两个实体都具有导航属性。 **当然** 是依赖实体，并且定义了 **DepartmentID** 外键属性。

![具有导航属性的部门和课程表](~/ef6/media/relationshipefdesigner.png)

下面的代码片段显示了用 Code First 创建的同一模型。

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>配置或映射关系

本页的其余部分介绍如何使用关系访问和操作数据。 有关设置模型中的关系的信息，请参阅以下页面。

-   若要在 Code First 中配置关系，请参阅 [数据批注](xref:ef6/modeling/code-first/data-annotations) 和 [熟知的 API-关系](xref:ef6/modeling/code-first/fluent/relationships)。
-   若要使用 Entity Framework Designer 配置关系，请参阅 [与 EF 设计器](xref:ef6/modeling/designer/relationships)之间的关系。

## <a name="creating-and-modifying-relationships"></a>创建和修改关系

在 *外键关联*中，当你更改关系时，状态为的依赖对象的状态将 `EntityState.Unchanged` 更改为 `EntityState.Modified` 。 在独立关系中，更改关系不会更新依赖对象的状态。

下面的示例演示如何使用外键属性和导航属性将相关对象关联起来。 使用外键关联，可以使用这两种方法更改、创建或修改关系。 使用独立关联，则不能使用外键属性。

- 将新值分配给外键属性，如下面的示例中所示。  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- 下面的代码通过将外键设置为 **null**来移除关系。 请注意，外键属性必须可以为 null。  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > 如果引用处于 "已添加" 状态 (在此示例中，课程对象) ，则在调用 SaveChanges 之前，引用导航属性将不会与新对象的键值同步。 由于对象上下文在键值保存前不包含已添加对象的永久键，因此不发生同步。 如果在设置关系后，必须完全同步新对象，请使用以下方法之一。

- 通过将一个新对象分配给导航属性。 下面的代码创建一个课程与之间的关系 `department` 。 如果将对象附加到上下文，则 `course` 也会将添加到 `department.Courses` 集合中，对象上的相应外键属性 `course` 将设置为该部门的键属性值。  
  ``` csharp
  course.Department = department;
  ```

- 若要删除关系，请将导航属性设置为 `null` 。 如果使用的是基于 .NET 4.0 的实体框架，则需要加载相关端，然后将其设置为 null。 例如：   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  从基于 .NET 4.5 实体框架5.0 开始，可以在不加载相关端的情况下将关系设置为 null。 还可以使用以下方法将当前值设置为 null。   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- 通过在实体集合中删除或添加对象。 例如，可以将类型的对象添加 `Course` 到 `department.Courses` 集合中。 此操作在特定的 **课程** 与特定的之间建立关系 `department` 。 如果将对象附加到上下文，则会将 **课程** 对象上的部门引用和外键属性设置为相应的 `department` 。  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- 通过使用 `ChangeRelationshipState` 方法更改两个实体对象之间的指定关系的状态。 此方法最常用于使用 N 层应用程序和 *独立关联* ， (不能将其与外键关联) 一起使用。 此外，若要使用此方法，必须将下拉到 `ObjectContext` ，如下面的示例中所示。  
在下面的示例中，讲师和课程之间存在多对多的关系。 调用 `ChangeRelationshipState` 方法并传递 `EntityState.Added` 参数，可以 `SchoolContext` 了解在两个对象之间添加了关系：
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  请注意，如果您正在更新 (不只是添加) 关系，则必须在添加新关系之后删除旧关系：

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>同步外键和导航属性之间的更改

使用上述方法之一更改附加到上下文的对象的关系时，实体框架需要使外键、引用和集合保持同步。实体框架会自动管理此同步 (也称为具有代理的 POCO 实体的关系修补) 。 有关详细信息，请参阅使用 [代理](xref:ef6/fundamentals/proxies)。

如果使用不带代理的 POCO 实体，则必须确保调用 **DetectChanges** 方法来同步上下文中的相关对象。 请注意，以下 Api 会自动触发 **DetectChanges** 调用。

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   对执行 LINQ 查询 `DbSet`

## <a name="loading-related-objects"></a>正在加载相关对象

在实体框架通常使用导航属性来加载通过定义的关联与返回的实体相关的实体。 有关详细信息，请参阅 [加载相关对象](xref:ef6/querying/related-data)。

> [!NOTE]
> 在外键关联中，加载依赖对象的相关端时，将会基于当前位于内存中的依赖对象的外键值加载相关对象：

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course = context.Courses.First(c => c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

在独立关联中，基于当前数据库中的外键值查询依赖对象的相关端。 但是，如果关系已修改，且依赖对象的引用属性指向对象上下文中加载的其他主体对象，实体框架将尝试创建关系，因为它是在客户端上定义的。

## <a name="managing-concurrency"></a>管理并发

在外键和独立关联中，并发检查基于在模型中定义的实体键和其他实体属性。 使用 EF 设计器创建模型时，请将属性设置 `ConcurrencyMode` 为 " **固定** "，以指定应检查属性的并发性。 使用 Code First 定义模型时，请 `ConcurrencyCheck` 在要检查其并发性的属性上使用批注。 使用 Code First 时，还可以使用 `TimeStamp` 批注来指定应检查属性的并发性。 给定类中只能有一个时间戳属性。 Code First 将此属性映射到数据库中不可以为 null 的字段。

建议你始终在使用参与并发检查和解析的实体时使用外键关联。

有关详细信息，请参阅 [处理并发冲突](xref:ef6/saving/concurrency)。

## <a name="working-with-overlapping-keys"></a>使用重叠键

重叠键是指键中某些属性亦是实体中其他键的一部分的那些组合键。 在独立关联中不能包含重叠键。 若要更改包含重叠键的外键关联，我们建议您修改外键值而不要使用对象引用。
