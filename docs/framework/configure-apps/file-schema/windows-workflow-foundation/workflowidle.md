---
title: <workflowIdle>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: b2ef703c-3e01-4213-9d2e-c14c7dba94d2
ms.openlocfilehash: 790e852eb515e19afc324f6e1c25db81ed22999c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148667"
---
# \<workflowIdle>

一种服务行为，可以控制何时卸载和持久保存空闲工作流实例。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<workflowIdle>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="String">
      <workflowIdle timeToPersist="TimeSpan"
                    timeToUnload="TimeSpan" />
    </behavior>
  </serviceBehaviors>
</behaviors>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|timeToPersist|一个 Timespan 值，该值指定工作流进入空闲状态与持久保存工作流之间的持续时间。 默认值为 TimeSpan.MaxValue。<br /><br /> 该持续时间从工作流实例进入空闲状态时算起。 如果您想要更积极地持久保存工作流实例，同时还要尽可能长久地在内存中保留该实例，此特性会非常有用。 仅当此属性的值小于 **timeToUnload** 属性时，此属性才有效。 如果大于该特性，将忽略此项。 如果此属性在 **timeToUnload** 属性指定的值之前过期，则必须在卸载工作流之前完成暂留。 这意味着卸载操作可能要等到工作流永久保留完毕才能进行。 永久层负责处理暂时性错误的任何重试操作，并仅在发生不可恢复的错误时引发异常。 因此，任何在持久性操作期间引发的异常都将被视为是致命的，并导致该工作流实例中止。|  
|timeToUnload|一个 Timespan 值，该值指定工作流进入空闲状态与卸载工作流之间的持续时间。 默认值为 1 分钟。<br /><br /> 卸载一个工作流意味着该工作流已持久保存。 如果此特性设置为零，则在工作流进入空闲状态后会立即持久保存并卸载该工作流实例。 将此特性设置为 TimeSpan.MaxValue 可以有效地禁用卸载操作。 处于空闲状态的工作流实例永远不会被卸载。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceBehaviors> 的 \<behavior>](behavior-of-servicebehaviors-of-workflow.md)|指定行为元素。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior>
- <xref:System.ServiceModel.Activities.Configuration.WorkflowIdleElement>
