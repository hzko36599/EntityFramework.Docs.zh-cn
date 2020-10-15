---
title: 基本保存 - EF Core
description: 关于通过 Entity Framework Core 添加、更新和删除数据的基本信息
author: ajcvickers
ms.date: 10/27/2016
uid: core/saving/basic
ms.openlocfilehash: c6288fc8ca79b78b6c377f25e3874211b38059b2
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063629"
---
# <a name="basic-save"></a>基本保存

了解如何使用上下文和实体类添加、修改和删除数据。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/)。

## <a name="adding-data"></a>添加数据

使用 *DbSet.Add* 方法添加实体类的新实例。 调用 *SaveChanges* 时，数据将插入到数据库中。

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> 添加、附加和更新方法全部呈现在传递给这些方法的实体的完整关系图上，如[相关数据](xref:core/saving/related-data)部分中所述。 此外，还可以使用 EntityEntry.State 属性仅设置单个实体的状态。 例如，`context.Entry(blog).State = EntityState.Modified`。

## <a name="updating-data"></a>更新数据

EF 将自动检测对由上下文跟踪的现有实体所做的更改。 这包括从数据库加载/查询的实体，以及之前添加并保存到数据库的实体。

只需修改分配给属性的值，然后调用 *SaveChanges*。

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>删除数据

使用 DbSet.Remove** 方法删除实体类的实例。

如果实体已存在于数据库中，则将在“SaveChanges”** 期间删除该实体。 如果实体尚未保存到数据库（即跟踪为“已添加”），则在调用“SaveChanges”** 时，该实体会从上下文中删除且不再插入。

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>单个 SaveChanges 中的多个操作

可以将多个添加/更新/删除操作合并到对“SaveChanges”** 的单个调用。

> [!NOTE]  
> 对于大多数数据库提供程序，“SaveChanges”** 是事务性的。 这意味着所有操作将成功或失败，决不部分应用这些操作。

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
