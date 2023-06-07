---
title: 入门 Tutorial2
description: 本文将开始一系列教程，其中介绍了如何对 Windows Workflow Foundation 应用程序进行编程。
ms.date: 03/30/2017
helpviewer_keywords:
- WF [WF], getting started
- Windows Workflow Foundation [WF], getting started
ms.assetid: c2d3585f-6b1a-4d4f-9865-bd7cd31c5d42
ms.openlocfilehash: e9856320faa82becf12dda04d02f6f1c08081feb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293593"
---
# <a name="getting-started-tutorial"></a>入门教程

本部分包含一组演练主题，介绍如何对 Windows Workflow Foundation (WF) 应用程序进行编程。 按照这些主题中的过程操作，将生成一个猜数游戏应用程序。 本教程中的第一个主题将逐步引导您创建工作流所需的自定义活动。 在第二个主题中，将这些活动与内置的工作流活动组合成一个流程图工作流。 在第三个主题中，配置主机应用程序以运行工作流，并在最后的主题中介绍持久性。 此过程的每一步骤都依赖于前面的步骤，因此建议您按顺序完成这些步骤。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：创建活动](how-to-create-an-activity.md)  
 介绍如何创建从 <xref:System.Activities.NativeActivity%601> 派生的自定义活动，以及如何使用活动设计器将此活动与内置的活动组合成一个复合活动。  
  
 [如何：创建工作流](how-to-create-a-workflow.md)  
 通过使用内置的活动和前面教程中的自定义活动，介绍如何创建流程图、顺序工作流和状态机工作流。  
  
 [如何：运行工作流](how-to-run-a-workflow.md)  
 介绍如何从宿主环境调用工作流、如何将数据传入和传出工作流以及如何继续书签。  
  
 [如何：创建和运行长期运行的工作流](how-to-create-and-run-a-long-running-workflow.md)  
 介绍如何向工作流应用程序添加持久性。  
  
 [如何：创建自定义跟踪参与者](how-to-create-a-custom-tracking-participant.md)  
 介绍如何创建自定义跟踪参与者和跟踪配置文件。  
  
 [如何：并行承载多个版本的工作流](how-to-host-multiple-versions-of-a-workflow-side-by-side.md)  
 介绍如何使用 `WorkflowIdentity` 并行承载工作流的多个版本。  
  
 [如何：更新正在运行的工作流实例的定义](how-to-update-the-definition-of-a-running-workflow-instance.md)  
 介绍如何使用动态更新来修改运行的工作流实例。  
  
## <a name="see-also"></a>另请参阅

- [Windows Workflow Foundation 编程](programming.md)
