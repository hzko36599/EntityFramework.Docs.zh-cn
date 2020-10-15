---
title: Azure Cosmos DB 提供程序-限制-EF Core
description: 与其他提供程序相比，Entity Framework Core Azure Cosmos DB 提供程序的限制
author: AndriySvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: e4d1c38da14d1e722797f8543313a69c7fb088cc
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064032"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Azure Cosmos DB 提供程序限制 EF Core

Cosmos 提供程序有很多限制。 其中的许多限制是由基础 Cosmos 数据库引擎中的限制引起的，并不特定于 EF。 但大多数情况还 [没有实现](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc)。

## <a name="temporary-limitations"></a>临时限制

- 即使只有一个实体类型没有映射到容器，它仍具有鉴别器属性。
- 在某些情况下，具有分区键的实体类型不能正常工作
- `Include` 不支持调用
- `Join` 不支持调用

## <a name="azure-cosmos-db-sdk-limitations"></a>Azure Cosmos DB SDK 限制

- 仅提供 async 方法

> [!WARNING]
> 由于没有 EF Core 依赖的低级方法的同步版本，因此当前通过对返回的调用来实现相应的功能 `.Wait()` `Task` 。 这意味着，使用或等方法（ `SaveChanges` `ToList` 而不是其异步对应项）可能会导致应用程序中出现死锁

## <a name="azure-cosmos-db-limitations"></a>Azure Cosmos DB 限制

你可以查看 [Azure Cosmos DB 支持的功能](/azure/cosmos-db/modeling-data)的完整概述，与关系数据库相比，这些是最明显的区别：

- 不支持客户端启动的事务
- 一些跨分区查询要么不受支持，要么速度慢得多，具体取决于所涉及的运算符
