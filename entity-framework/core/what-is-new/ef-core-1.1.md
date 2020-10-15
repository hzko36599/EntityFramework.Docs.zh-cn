---
title: EF Core 1.1 中的新增功能 - EF Core
description: Entity Framework Core 1.1 中的更改和改进
author: ajcvickers
ms.date: 10/27/2016
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: a865270d0b1b690ed2596e7ed550463ab0877bca
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063460"
---
# <a name="new-features-in-ef-core-11"></a>EF Core 1.1 中的新增功能

## <a name="modeling"></a>建模

### <a name="field-mapping"></a>字段映射

允许配置属性的支持字段。 这对于只读属性或具有 Get/Set 方法（而不是属性）的数据很有用。

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>映射到 SQL Server 内存优化表

你可以指定实体映射到的表是内存优化表。 使用 EF Core 创建和维护基于模型的数据库时（使用迁移或 `Database.EnsureCreated()`），将为这些实体创建内存优化表。

## <a name="change-tracking"></a>更改跟踪

### <a name="additional-change-tracking-apis-from-ef6"></a>来自 EF6 的其他更改跟踪 API

如 `Reload`、`GetModifiedProperties`、`GetDatabaseValues` 等。

## <a name="query"></a>查询

### <a name="explicit-loading"></a>显式加载

允许在先前从数据库加载的实体上触发导航属性填充。

### <a name="dbsetfind"></a>DbSet.Find

提供一种基于主键值提取实体的简单方法。

## <a name="other"></a>其他

### <a name="connection-resiliency"></a>连接复原

自动重试失败的数据库命令。 当连接到 SQL Azure 时（其中瞬间失败非常普遍）该操作非常有用。

### <a name="simplified-service-replacement"></a>简化的服务替换

替换 EF 使用的内部服务更加简单。
