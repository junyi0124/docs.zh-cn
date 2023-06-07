---
title: <participants>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 560dd0bb-f9fb-423c-8857-2101a3654b06
ms.openlocfilehash: 51c7824a4dcbd95eac098d25e9a971514e2a1e0b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167036"
---
# \<participants>

配置一列跟踪参与者，它们侦听直接从运行时发出的跟踪记录，并按照它们的任何方式处理这些记录。 这包括写入特定输出（例如，文件、控制台、ETW）、处理/聚合记录或可能需要的任何其他组合。  
  
 有关工作流跟踪和跟踪参与者的详细信息，请参阅 [工作流跟踪和](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md) 跟踪和 [跟踪参与者](../../../windows-workflow-foundation/tracking-participants.md)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<participants>**  
  
## <a name="syntax"></a>语法  
  
```xml
<tracking>
  <participants>
    <add name="String"
         profileName="String"
         type="String" />
  </participants>
</tracking>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<add>](add-of-participants.md)|包含跟踪参与者的设置。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<tracking>](tracking.md)|表示一个配置节，用于定义工作流服务的跟踪设置。|  
  
## <a name="remarks"></a>备注  

 跟踪参与者用于获取从工作流发出的跟踪数据并将其存储在不同的媒体中。 同样，也可以在跟踪参与者中执行对跟踪记录的任何后续处理。  
  
 多个跟踪参与者可同时使用跟踪事件。 每个跟踪参与者都可与不同的跟踪配置文件关联。  
  
 提供了将跟踪记录写入 ETW 会话的标准跟踪参与者。 通过在配置文件中添加特定于跟踪的行为，可以对工作流服务配置参与者。 通过启用 ETW 跟踪参与者，可以在事件查看器中查看跟踪记录。 如果这不能满足您的需求，您还可以编写自定义跟踪参与者。  
  
## <a name="example"></a>示例  

 下面的配置示例演示在 Web.config 文件中配置的标准 ETW 跟踪参与者。  
  
 ETW 跟踪参与者用于将跟踪记录写入 ETW 的提供程序 Id 在部分中定义 **\<diagnostics>** 。 跟踪参与者具有一个与其关联的配置文件，用来指定跟踪参与者已订阅的跟踪记录。 这由元素的 **profileName** 属性定义 **\<add>** 。 一旦定义了这些操作，就会将跟踪参与者添加到 **\<etwTracking>** 服务行为中。 这会将所选跟踪参与者添加到工作流实例的扩展中，以便它们开始接收跟踪记录。  
  
```xml
<configuration>
  <system.web>
    <compilation targetFrameworkMoniker=".NETFramework,Version=v4.0"/>
  </system.web>
  <system.serviceModel>
    <diagnostics etwProviderId="52A3165D-4AD9-405C-B1E8-7D9A257EAC9F" />
    <tracking>
      <participants>
        <add name="EtwTrackingParticipant"
             type="System.Activities.Tracking.EtwTrackingParticipant, System.Activities, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"
             profileName="HealthMonitoring_Tracking_Profile"/>
      </participants>
    </tracking>
    <behaviors>
      <serviceBehaviors>
        <behavior>
          <etwTracking profileName="Sample Tracking Profile"/>  
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activities.Tracking.Configuration.TrackingSection>
- <xref:System.ServiceModel.Activities.Description.EtwTrackingBehavior>
- <xref:System.ServiceModel.Activities.Configuration.EtwTrackingBehaviorElement>
- [工作流跟踪](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [跟踪参与者](../../../windows-workflow-foundation/tracking-participants.md)
