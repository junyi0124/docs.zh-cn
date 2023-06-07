---
title: 如何：使用 WorkflowServiceHost 配置空闲行为
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1bb93652-d687-46ff-bff6-69ecdcf97437
ms.openlocfilehash: 8491fccee9f148412aed89280ccd20b315d25da6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257361"
---
# <a name="how-to-configure-idle-behavior-with-workflowservicehost"></a>如何：使用 WorkflowServiceHost 配置空闲行为

当工作流遇到必须由某种外部刺激恢复的书签时（例如，当工作流实例正在等待使用 <xref:System.ServiceModel.Activities.Receive> 活动传递消息时），将转入空闲状态。 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior> 行为允许您指定服务实例进入空闲状态与保留或卸载服务实例之间的时间。 它包含两个使您能够设置这些时间跨度的属性。 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A> 指定工作流服务实例进入空闲状态与保留工作流服务实例之间的时间跨度。 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> 指定工作流服务实例进入空闲状态与卸载工作流服务实例之间的时间跨度，其中，卸载意味着将实例保留到实例存储区中并从内存中将其删除。 本主题解释如何在配置文件中配置 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior> 。  
  
### <a name="to-configure-workflowidlebehavior"></a>配置 WorkflowIdleBehavior  
  
1. 将 <`workflowIdle`> 元素添加到 `behavior` <> 元素中的 <> 元素， `serviceBehaviors` 如下面的示例中所示。  
  
    ```xml  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="">  
          <workflowIdle timeToUnload="0:05:0" timeToPersist="0:04:0"/>
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
     `timeToUnload` 特性指定工作流服务实例进入空闲状态与卸载工作流服务之间的时间段。 `timeToPersist` 特性指定工作流服务实例进入空闲状态与保存该实例之间的时间段。 `timeToUnload` 默认值为 1 分钟。 `timeToPersist` 的默认值为 <xref:System.TimeSpan.MaxValue>。 如果要在内存中保留空闲实例，但是为实现可靠性而保持这些实例，请设置值以使 `timeToPersist` < `timeToUnload`。 如果要防止卸载空闲实例，请将 `timeToUnload` 设置为 <xref:System.TimeSpan.MaxValue>。 有关的详细信息 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior> ，请参阅 [工作流服务主机扩展性](workflow-service-host-extensibility.md)  
  
    > [!NOTE]
    > 上面的配置示例使用的是简化配置。 有关详细信息，请参阅 [简化配置](../simplified-configuration.md)。  
  
### <a name="to-change-idle-behavior-in-code"></a>在代码中更改空闲行为  
  
- 下面的示例以编程方式更改保持和卸载之前的等待时间。  
  
     [!code-csharp[Wf_SvcHost_Idle_persist#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/wf_svchost_idle_persist/cs/source.cs#1)]
     [!code-vb[Wf_SvcHost_Idle_persist#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/wf_svchost_idle_persist/vb/source.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- [工作流服务主机可扩展性](workflow-service-host-extensibility.md)
- [简化配置](../simplified-configuration.md)
- [工作流服务](workflow-services.md)
