---
title: <tracking>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: fd9b50ed-98a1-4518-836d-e4e02c670822
ms.openlocfilehash: 0b00780dedc15fe90163145f23c57f62369c401f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198738"
---
# \<tracking>

表示一个配置节，用于定义工作流服务的跟踪设置。  
  
 有关工作流跟踪及其配置的详细信息，请参阅工作流 [跟踪和跟踪](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md) 和 [配置工作流跟踪](../../../windows-workflow-foundation/configuring-tracking-for-a-workflow.md)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<tracking>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <tracking>
    <profiles>
      <participants>
        <add name="String"
             profileName="String"
             type="String" />
      </participants>
      <trackingProfile name="String">
        <workflow activityDefinitionId="String">
          <activityScheduledQueries>
            <activityScheduledQuery activityName="String"
                                    childActivityName="String"/>
          </activityScheduledQueries>
          <activityStateQueries>
            <activityStateQuery activityName="String" />
            <arguments>
              <argument name="String" />
            </arguments>
            <states>
              <state name="String"  />
            </states>
            <variables>
              <variable name="String" />
            </variables>
          </activityStateQueries>
          <bookmarkResumptionQueries>
            <bookmarkResumptionQuery name="String" />
          </bookmarkResumptionQueries>
          <cancelRequestQueries>
            <cancelRequestQuery activityName="String"
                                childActivityName="String"/>
          </cancelRequestQueries>
          <customTrackingQueries>
            <customTrackingQuery activityName="String"
                                 name="String"/>
          </customTrackingQueries>
          <faultPropagationQueries>
            <faultPropagationQuery activityName="String"
                                   faultHandlerActivityName="String" />
          </faultPropagationQueries>
          <workflowInstanceQueries>
            <workflowInstanceQuery>
              <states>
                <state name="String" />
              </states>
            </workflowInstanceQuery>
          </workflowInstanceQueries>
        </workflow>
      </trackingProfile>
    </profiles>
  </tracking>
</system.serviceModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<participants>](participants.md)|一个配置元素集合，这些元素定义了订阅跟踪记录的参与者。 跟踪参与者包含用于处理跟踪记录中负载的逻辑（例如，它们可以选择向某个文件中写入）。|  
|[\<trackingProfile>](trackingprofile.md)|一个跟踪配置文件，用于筛选从工作流实例发出的跟踪记录。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|system.ServiceModel|所有工作流配置元素的根元素。|  
  
## <a name="remarks"></a>备注  

 利用跟踪可以检查工作流的执行。 工作流跟踪基础结构检测工作流以发出反应执行期间关键事件的记录。 例如，工作流实例开始或完成时会发出跟踪记录。 跟踪还可以提取与工作流变量关联的相关业务数据。 例如，如果工作流表示一个订单处理系统，则可以提取订单 ID 以及跟踪记录。 一般来讲，启用 WF 跟踪便于对工作流的执行进行诊断或业务分析。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activities.Tracking.Configuration.TrackingSection?displayProperty=nameWithType>
- [工作流跟踪](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
