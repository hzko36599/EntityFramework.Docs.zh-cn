---
title: 定义 Dbset-EF6
description: 定义实体框架6中的 Dbset
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/dbsets
ms.openlocfilehash: dc5ad7f8b4ba32454c702f354b37223007e856e3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065220"
---
# <a name="defining-dbsets"></a>定义 Dbset
使用 Code First 工作流进行开发时，可定义一个派生 DbContext，用于表示与数据库的会话，并为模型中的每个类型公开一个 DbSet。 本主题介绍可用于定义 DbSet 属性的各种方式。  

## <a name="dbcontext-with-dbset-properties"></a>具有 DbSet 属性的 DbContext  

Code First 示例中所示的常见情况是，对于模型的实体类型，DbContext 具有公共自动 DbSet 属性。 例如：  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

当在 Code First 模式下使用时，这会将博客和公告配置为实体类型，并配置可从此访问的其他类型。 此外，DbContext 会自动为每个属性调用 setter，以设置相应 DbSet 的实例。  

## <a name="dbcontext-with-idbset-properties"></a>具有 IDbSet 属性的 DbContext  

在某些情况下，例如创建模拟或 fakes 时，使用接口来声明集属性更有用。 在这种情况下，可以使用 IDbSet 接口代替 DbSet。 例如：  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

此上下文的工作方式与使用 DbSet 类作为其设置属性的上下文完全相同。  

## <a name="dbcontext-with-read-only-set-properties"></a>具有只读 set 属性的 DbContext  

如果你不希望为 DbSet 或 IDbSet 属性公开公共资源库，则可以改为创建只读属性并自行创建集实例。 例如：  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

请注意，DbContext 将缓存从 Set 方法返回的 DbSet 的实例，使每个属性在每次调用时都返回相同的实例。  

Code First 的实体类型的发现的工作方式与使用公共 getter 和 setter 的属性相同。  
