---
title: 每个模型多个关系图-EF6
description: 实体框架6中的每个模型多个关系图
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/multiple-diagrams
ms.openlocfilehash: 625cb932d23805f6bd22b658e4cddab1a487751c
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066208"
---
# <a name="multiple-diagrams-per-model"></a>每个模型多个关系图
> [!NOTE]
> **EF5 仅向前** -实体框架5中引入了本页中所述的功能、api 等。 如果使用的是早期版本，则部分或全部信息不适用。

此视频和页面显示了如何使用 Entity Framework Designer (EF 设计器) 将模型拆分为多个关系图。 当模型变得太大，无法查看或编辑时，可能需要使用此功能。

在 EF 设计器的早期版本中，每个 EDMX 文件只能有一个关系图。 从 Visual Studio 2012 开始，可以使用 EF 设计器将 EDMX 文件拆分成多个关系图。

## <a name="watch-the-video"></a>观看视频
此视频演示如何使用 Entity Framework Designer (EF 设计器) 将模型拆分为多个关系图。 当模型变得太大，无法查看或编辑时，可能需要使用此功能。

**提供者**： Julia Kornich

**视频**： [wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv)  |  [.wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v)  |  [wmv (ZIP) ](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>EF 设计器概述

使用 EF 设计器的实体数据模型向导创建模型时，将创建一个 .edmx 文件并将其添加到解决方案中。 此文件定义了你的实体的形状，以及它们如何映射到数据库。

EF 设计器包含以下组件：

-   用于编辑模型的可视化设计图面。 您可以创建、修改或删除实体和关联。
-   提供模型的树视图的 **模型浏览器**   窗口。实体及其关联位于* \[ ModelName \] *文件夹下。 数据库表和约束位于* \[ ModelName \] *下。存储文件夹。
-   用于查看和编辑映射的 " **映射详细信息** " 窗口。 您可以将实体类型或关联映射到数据库表、列和存储过程。 

实体数据模型向导完成时，将自动打开 "可视化设计图面" 窗口。 如果 "模型浏览器" 不可见，请右键单击主设计图面，然后选择 " **模型浏览器**"。

以下屏幕截图显示了在 EF 设计器中打开的 .edmx 文件。 屏幕截图显示左侧)  (的可视化设计图面，而 **模型浏览器**   窗口 (向右) 。

![EF 设计器2](~/ef6/media/efdesigner2.png)

若要撤消在 EF 设计器中完成的操作，请单击 "Ctrl + Z"。

## <a name="working-with-diagrams"></a>使用关系图

默认情况下，EF 设计器会创建一个名为 Diagram1 的关系图。 如果你有一个具有大量实体和关联的关系图，则你最喜欢按逻辑拆分它们。 从 Visual Studio 2012 开始，可以在多个关系图中查看概念模型。   

添加新关系图时，它们将显示在 "模型浏览器" 窗口中的 "关系图" 文件夹下。 重命名关系图：在 "模型浏览器" 窗口中选择关系图，单击 "名称"，然后键入新名称。 您还可以右键单击关系图名称，然后选择 " **重命名**"。

关系图名称将显示在 Visual Studio 编辑器中的 .edmx 文件名旁。 例如 Model1 \[ Diagram1 \] 。

![关系图名称](~/ef6/media/diagramname.png)

关系图内容 (实体和关联的形状和颜色) 存储在 .edmx 文件中。 若要查看此文件，请选择解决方案资源管理器并展开 .edmx 文件。 

![关系图文件](~/ef6/media/diagramfiles.png)

不应手动编辑 .edmx 文件，此文件的内容可能会被 EF 设计器覆盖。
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>将实体和关联拆分为新关系图

您可以在现有关系图上选择实体 (按住 Shift 来选择多个实体) 。 单击鼠标右键并选择 " **移到新关系图**"。 将创建新关系图，并将所选实体及其关联移动到关系图中。

或者，您可以在模型浏览器中右键单击 "关系图" 文件夹，然后选择 " **添加新关系图"。** 然后，可以将实体从模型浏览器中的 "实体类型" 文件夹下拖放到设计图面上。

你还可以使用 Ctrl X 或 Ctrl-c 键) 从一个关系图中剪切或复制 (实体，然后使用 Ctrl + V 键) 在另一个关系图中粘贴 (。 如果要将实体粘贴到其中的关系图已经包含具有相同名称的实体，则将创建一个新实体并将其添加到模型中。例如： Diagram2 包含 "部门" 实体。 然后，将其他部门粘贴到 Diagram2 上。 将创建 Department1 实体并将其添加到概念模型中。   

若要在关系图中包含相关实体，则 rick-单击该实体并选择 " **包括相关**项"。 这将在指定的关系图中创建相关实体和关联的副本。

## <a name="changing-the-color-of-entities"></a>更改实体的颜色

除了将模型拆分为多个关系图外，还可以更改实体的颜色。

若要更改颜色，请在设计图面上选择一个实体 (或多个实体) 。 然后，单击鼠标右键并选择 " **属性**"。 在属性窗口中，选择 " **填充颜色** " 属性。 使用有效的颜色名称指定颜色 (例如，红色) 或有效的 RGB (例如，255、128、128) 。 

![更改颜色](~/ef6/media/color.png)

## <a name="summary"></a>摘要

在本主题中，我们介绍如何将模型拆分为多个关系图，以及如何使用 Entity Framework Designer 为实体指定不同的颜色。 
