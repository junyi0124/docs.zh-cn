---
title: 使用 Activity 类的工作流活动创作
ms.date: 03/30/2017
ms.assetid: 7b7b1c66-f093-43c3-b4d1-7173b46516da
ms.openlocfilehash: 21f1c8b1249d41029fa7a19360e96ad866c823a7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293840"
---
# <a name="workflow-activity-authoring-using-the-activity-class"></a>使用 Activity 类的工作流活动创作

使用 Windows Workflow Foundation (WF) 创建活动的最基本方法 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] 是创建一个继承自的类，该类 <xref:System.Activities.Activity> 通过从 [内置活动库](net-framework-4-5-built-in-activity-library.md)中组合自定义活动或活动来创建功能。 本主题演示如何创建向控制台写入两个消息的活动。

### <a name="to-create-a-custom-activity-using-the-activity-designer"></a>使用活动设计器创建自定义活动

1. 打开 Visual Studio 2012。

2. 依次选择“文件”、“新建”和“项目”。 在 "**项目类型**" 窗口中的 " **Visual c #** " 下选择 " **Workflow 4.0** "，然后选择 " **v2010** " 节点。 在 "**模板**" 窗口中选择 "**活动库**"。 将新项目命名为 HelloActivity。

3. 打开新的活动。  将 <xref:System.Activities.Statements.Sequence> 活动从工具箱拖到设计器图面。

4. 将 <xref:System.Activities.Statements.WriteLine> 活动拖到 <xref:System.Activities.Statements.Sequence> 活动中。 `"Hello World"`在 "**文本**" 字段中输入引号)  (。

5. 将第二个 <xref:System.Activities.Statements.WriteLine> 活动拖到 <xref:System.Activities.Statements.Sequence> 活动中并将其放置在第一个活动的下面。 `"Goodbye"`在 "**文本**" 字段中输入引号)  (。
