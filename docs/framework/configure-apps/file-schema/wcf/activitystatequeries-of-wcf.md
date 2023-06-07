---
title: <activityStateQueries>WCF 的
ms.date: 10/08/2018
ms.assetid: 9e45db49-ed85-4fdf-bd65-0d5477e31823
ms.openlocfilehash: 249ac3d91f6251a943dd856e4122b8b54f691702
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850571"
---
# <a name="activitystatequeries-of-wcf"></a>\<activityStateQueries>WCF 的

表示一个查询集合，这些查询用于跟踪构成工作流实例的活动的生命周期更改。 例如，你可能想要跟踪每次在工作流实例中完成 "发送电子邮件" 活动的时间。 跟踪参与者需要用此查询来订阅活动状态记录对象。 在 ActivityStates 中指定了要订阅的可用状态。

有关跟踪配置文件查询的详细信息，请参阅[跟踪配置文件](../../../windows-workflow-foundation/tracking-profiles.md)。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<profiles>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<trackingProfile>**](trackingprofile-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<workflow>**](workflow-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<activityStateQueries>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<tracking>
  <profiles>
    <trackingProfile name="Name">
      <workflow>
        <activityStateQueries>
          <activityStateQuery activityName="String">
            <arguments>
              <argument name="String" />
            </arguments>
            <states>
              <state name="String" />
            </states>
            <variables>
              <variable name="String" />
            </variables>
          </activityStateQuery>
        </activityStateQueries>
      </workflow>
    </trackingProfile>
  </profiles>
</tracking>
```  

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。
  
### <a name="attributes"></a>特性  

无。  

### <a name="child-elements"></a>子元素

|元素|说明|
|-------------|-----------------|
|[\<activityStateQuery>](activitystatequery-of-wcf.md)|一个查询，用于跟踪在活动中发生的错误的处理。  每次 FaultHandler 处理错误时，都会发生此事件。|

### <a name="parent-elements"></a>父元素

|元素|说明|
|-------------|-----------------|
|[\<workflow>](../windows-workflow-foundation/workflow.md)|一个配置元素，包含 `activityDefinitionId` 属性所标识的特定工作流的所有查询。|

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Activities.Tracking.Configuration.ActivityStateQueryElementCollection>
- <xref:System.Activities.Tracking.ActivityStateQuery>
- [工作流跟踪](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [跟踪配置文件](../../../windows-workflow-foundation/tracking-profiles.md)
