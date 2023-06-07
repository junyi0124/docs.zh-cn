---
title: 如何：使用 WorkflowServiceHost 配置跟踪
ms.date: 03/30/2017
ms.assetid: ed1485fe-7529-4351-bca3-8bb915260b17
ms.openlocfilehash: cf30ace90f86e282d72c4da5f2c3707905360aeb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257348"
---
# <a name="how-to-configure-tracking-with-workflowservicehost"></a>如何：使用 WorkflowServiceHost 配置跟踪

本主题说明如何为 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 中承载的 <xref:System.ServiceModel.Activities.WorkflowServiceHost>工作流配置跟踪。 可以通过指定服务行为使用 Web.config 文件进行配置。  
  
### <a name="configure-tracking-in-configuration"></a>在配置中配置跟踪  
  
1. <xref:System.Activities.Tracking.EtwTrackingParticipant> `behavior` 在配置文件中使用 <> 元素添加，如下面的示例中所示。  
  
    ```xml  
    <behaviors>  
       <serviceBehaviors>  
         <behavior>  
           <etwTracking profileName="Sample Tracking Profile" />  
         </behavior>
       </serviceBehaviors>  
    </behaviors>  
    ```  
  
    > [!NOTE]
    > 上面的配置示例使用的是简化配置。 有关详细信息，请参阅 [简化配置](../simplified-configuration.md)。  
  
     上面的配置示例添加一个 <xref:System.Activities.Tracking.EtwTrackingParticipant>，并指定一个跟踪配置文件名称。 跟踪配置文件是在 `trackingProfile` <> 元素内的 <> 元素中创建的 `tracking` 。 跟踪配置文件包含跟踪查询，这些查询允许跟踪参与者订阅工作流实例的状态在运行时发生更改时发出的工作流事件。 下面的示例演示如何创建跟踪配置文件。  
  
    ```xml  
    <system.serviceModel>  
        <tracking>
         <trackingProfile name="Sample Tracking Profile">  
            <workflow activityDefinitionId="*">  
               <workflowInstanceQueries>  
                 <workflowInstanceQuery>  
                    <states>  
                       <state name="Started"/>  
                       <state name="Completed"/>  
                    </states>  
                </workflowInstanceQuery>  
             </workflowInstanceQueries>  
           </workflow>  
         </trackingProfile>
       </tracking>  
    </system.serviceModel>  
    ```  
  
     有关跟踪配置文件的详细信息，请参阅 [跟踪配置文件](../../windows-workflow-foundation/tracking-profiles.md)。  
  
     有关常规跟踪的详细信息，请参阅 [工作流跟踪和跟踪](../../windows-workflow-foundation/workflow-tracking-and-tracing.md)。  
  
### <a name="configure-tracking-in-code"></a>在代码中配置跟踪  
  
1. 在代码中使用 <xref:System.Activities.Tracking.EtwTrackingParticipant> 行为添加 <xref:System.ServiceModel.Activities.Description.EtwTrackingBehavior>，如下面的示例所示。  
  
    ```csharp  
    host.Description.Behaviors.Add(new EtwTrackingBehavior { ProfileName = "Sample Tracking Profile" });  
    ```  
  
     上面的代码示例添加一个 <xref:System.Activities.Tracking.EtwTrackingParticipant>，并指定一个跟踪配置文件名称。 跟踪配置文件是在 `trackingProfile` <> 元素内的 <> 元素中创建的 `tracking` ，如上一节所示。  
  
     有关跟踪配置文件的详细信息，请参阅 [跟踪配置文件](../../windows-workflow-foundation/tracking-profiles.md)。  
  
     有关常规跟踪的详细信息，请参阅 [工作流跟踪和跟踪](../../windows-workflow-foundation/workflow-tracking-and-tracing.md)。 有关以编程方式配置跟踪的示例，请参阅为 [工作流配置跟踪](../../windows-workflow-foundation/configuring-tracking-for-a-workflow.md)。  
  
## <a name="see-also"></a>另请参阅

- [WCF 服务的简化配置](../samples/simplified-configuration-for-wcf-services.md)
- [工作流服务](workflow-services.md)
- [跟踪配置文件](../../windows-workflow-foundation/tracking-profiles.md)
