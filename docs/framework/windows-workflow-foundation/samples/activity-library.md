---
title: 活动库
ms.date: 03/30/2017
ms.assetid: 5323e9d4-71d6-47eb-bfa6-31feac62044d
ms.openlocfilehash: 1a0c289315d7181645573098916788f18493abb8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245688"
---
# <a name="activity-library"></a>活动库

本部分包含的示例演示了 Windows Workflow Foundation (WF) 中的高级自定义活动。  
  
## <a name="in-this-section"></a>本节内容

 [SendMail 自定义活动](sendmail-custom-activity.md)  
 演示如何创建派生自 <xref:System.Activities.AsyncCodeActivity> 的自定义活动，以使用 SMTP 发送邮件供在工作流应用程序内使用。  
  
 [限制并行 ForEach](throttled-parallel-foreach.md)  
 演示 `ThrottleParallelForEach` 活动与 <xref:System.Activities.Statements.ParallelForEach%601> 活动的相似之处，二者的不同之处在于，前者允许设置一个并发因子来限制要同时执行的分支的数量。
  
 [数据库访问活动](database-access-activities.md)  
 演示如何创建活动，以允许访问数据库以检索或修改信息，并使用 [ADO.NET](../../data/adonet/index.md) 来访问数据库。  
  
 [.NET Framework 4.5 中的外部化策略活动](externalized-policy-activity-in-net-framework-4-5.md)  
 演示 ExternalizedPolicy4 活动如何 <xref:System.Workflow.Activities.Rules.RuleSet> [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 通过使用在 wf 3.5 中随附的规则引擎，在 Windows Workflow Foundation wf 4.5 (中，通过) 中的 wf 直接执行 .NET Framework 3.5 (WF) 3.5 中的现有 Windows Workflow Foundation。
  
 [非泛型 ForEach](non-generic-foreach.md)  
 演示了如何创建非泛型版本的 <xref:System.Activities.Statements.ForEach%601> 活动。  
  
 [非泛型 ParallelForEach](non-generic-parallelforeach.md)  
 演示了如何创建非泛型版本的 <xref:System.Activities.Statements.ParallelForEach%601> 活动。  
  
 [获取 WorkflowInstanceId](get-workflowinstanceid.md)  
 演示如何使用自定义活动 `GetWorkflowInstanceId` 返回工作流实例 ID。
