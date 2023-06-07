---
title: <behavior><serviceBehaviors>工作流的
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 6a4b718a-1b40-4957-935a-f6122819ab3c
ms.openlocfilehash: 14c528746963a3078e0ab377d095414d2fca0dbe
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189612"
---
# <a name="behavior-of-servicebehaviors-of-workflow"></a>\<behavior>\<serviceBehaviors>工作流的

**行为**元素包含服务行为的设置集合。 每个行为都按其 **名称**编制索引。 服务可以使用元素的 **behaviorConfiguration** 特性通过此名称链接到每个行为 [\<endpoint>](../wcf/endpoint-element.md) 。 这样，终结点可以共享公共行为配置而不用重新定义设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<behavior>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.ServiceModel>  
  <behaviors>  
    <serviceBehaviors>  
      <behavior name="String">
        <bufferReceive maxPendingMessagesPerChannel="Integer" />
        <etwTracking profileName="String" />
        <sendMessageChannelCache allowUnsafeCaching="Boolean">
          <channelSettings idleTimeout="TimeSpan"
                           leaseTimeout="TimeSpan"
                           maxItemsInCache="Integer" />
          <factorySettings idleTimeout="TimeSpan"
                           leaseTimeout="TimeSpan"
                           maxItemsInCache="Integer" />
        </sendMessageChannelCache>
        <sqlWorkflowInstanceStore connectionStringName="String"
                                  hostLockRenewalPeriod="TimeSpan"
                                  instanceCompletionAction="DeleteNothing/DeleteAll"
                                  instanceEncodingAction="None/GZip"
                                  instanceLockedExceptionAction="NoRetry/BasicRetry/AggressiveRetry"
                                  runnableInstancesDetectionPeriod="TimeSpan" />
        <workflowIdle timeToPersist="TimeSpan"
                      timeToUnload="TimeSpan" />
        <workflowUnhandledException action="Abandon/AbandonAndSuspend/Cancel/Terminate" />
      </behavior>
    </serviceBehaviors>  
  </behaviors>  
</system.ServiceModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|name|一个包含行为的配置名称的唯一字符串。 此值是用户定义的一个字符串，该字符串必须是唯一的，因为它将充当元素的标识字符串。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bufferReceive>](bufferreceive.md)|一种服务行为，允许服务使用缓冲接收处理，以使工作流服务能够处理无序消息。|  
|[\<routing>](../wcf/routing-of-servicebehavior.md)|一种服务行为，允许服务使用 <xref:System.Activities.Tracking.EtwTrackingParticipant> 来利用 ETW 跟踪。|  
|[\<sendMessageChannelCache>](sendmessagechannelcache.md)|一种服务行为，该行为支持缓存共享级别的自定义、通道工厂缓存的设置，以及使用 Send 消息传递活动将消息发送给服务终结点的工作流通道缓存的设置。|  
|[\<sqlWorkflowInstanceStore>](sqlworkflowinstancestore.md)|一种服务行为，它允许您配置 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 功能，该功能支持将工作流服务实例的状态信息保持到 SQL Server 2005 或 SQL Server 2008 数据库中。|  
|[\<workflowIdle>](workflowidle.md)|一种服务行为，可以控制何时卸载和持久保存空闲工作流实例。|  
|[\<workflowInstanceManagement>](workflowinstancemanagement.md)|一种服务行为，可用于指定控制工作流实例如何运行的设置，包括持久性、未经处理的异常行为和空闲行为。|  
|[\<workflowUnhandledException>](workflowunhandledexception.md)|一种服务行为，可用于指定工作流服务中发生未经处理的异常时所采取的操作。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceBehaviors>](servicebehaviors-of-workflow.md)|服务行为元素的集合。|
