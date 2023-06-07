---
title: 如何：使用 WorkflowServiceHost 配置工作流未经处理的异常行为
ms.date: 03/30/2017
ms.assetid: 51b25c86-292c-43e4-8d13-273d2badc8ad
ms.openlocfilehash: 93eb2f4493b70f54336a5d47957c6913239088e5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264850"
---
# <a name="how-to-configure-workflow-unhandled-exception-behavior-with-workflowservicehost"></a>如何：使用 WorkflowServiceHost 配置工作流未经处理的异常行为

<xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> 行为可用于指定 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 承载的工作流中出现未经处理的异常时所采取的操作。 本主题演示如何在配置文件中配置此行为。  
  
### <a name="to-configure-workflowunhandledexceptionbehavior"></a>配置 WorkflowUnhandledExceptionBehavior  
  
1. `workflowUnhandledException`在 <> 元素内的 <> 元素中添加一个 <> 元素 `behavior` `serviceBehaviors` ，使用 `action` 特性指定发生未经处理的异常时要执行的操作，如下面的示例中所示。  
  
    ```xml  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="">  
          <workflowUnhandledException action="abandonAndSuspend"/>
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
    > [!NOTE]
    > 上面的配置示例使用的是简化配置。 有关详细信息，请参阅 [简化配置](../simplified-configuration.md)。  
  
     可在代码中配置该行为，如下面的示例所示。  
  
    ```csharp  
    host.Description.Behaviors.Add(new WorkflowUnhandledExceptionBehavior { Action = WorkflowUnhandledExceptionAction.AbandonAndSuspend });  
    ```  
  
     `action` `workflowUnhandledException` 可以将 <> 元素的属性设置为以下值之一：  
  
     **终止**  
     中止内存中的实例，而不影响保存的实例状态（即回滚到上一个保存点）。  
  
     **abandonAndSuspend**  
     中止内存中的实例并将保存的实例更新为挂起。  
  
     **cancel**  
     为实例调用取消处理程序，然后在内存中完成该实例，此操作也有可能将实例从实例存储区中移除。  
  
     **终止**  
     在内存中完成实例并将其从实例存储区中移除。  
  
     有关的详细信息 <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> ，请参阅 [工作流服务主机扩展性](workflow-service-host-extensibility.md)。  
  
## <a name="see-also"></a>另请参阅

- [工作流服务主机可扩展性](workflow-service-host-extensibility.md)
- [工作流服务](workflow-services.md)
