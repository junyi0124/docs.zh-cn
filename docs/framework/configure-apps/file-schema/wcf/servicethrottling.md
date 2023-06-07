---
title: <serviceThrottling>
ms.date: 03/30/2017
ms.assetid: a337d064-1e64-4209-b4a9-db7fdb7e3eaf
ms.openlocfilehash: 0c6d844ac287037b7a546d3a48e7cd924e8a63d1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153607"
---
# \<serviceThrottling>

指定 Windows Communication Foundation (WCF) 服务的限制机制。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceThrottling>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<serviceThrottling maxConcurrentCalls="Integer"
                   maxConcurrentInstances="Integer"
                   maxConcurrentSessions="Integer" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|maxConcurrentCalls|一个正整数，用于限制当前在整个 <xref:System.ServiceModel.ServiceHost> 中处理的消息数目。 超出此限制的调用将在队列中排队。 将此值设置为 0 与将其设置为 Int32.MaxValue 等效。 默认值是 16 * 处理器计数。|  
|maxConcurrentInstances|一个正整数，用于限制在整个 <xref:System.ServiceModel.InstanceContext> 中一次执行的 <xref:System.ServiceModel.ServiceHost> 对象数。 用于创建其他实例的请求将会排队，并在出现低于该限值的槽时完成。 默认值是 maxConcurrentSessions 和 MaxConcurrentCalls 的和|  
|maxConcurrentSessions|一个正整数，用于限制 <xref:System.ServiceModel.ServiceHost> 对象可以接受的会话数。<br /><br /> 此服务将接受超出限制的连接，但是，只有处于限制范围之内的通道处于活动状态（会从此通道中读取消息）。 将此值设置为 0 与将其设置为 Int32.MaxValue 等效。 默认值是 100 * 处理器计数。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|指定行为元素。|  
  
## <a name="remarks"></a>备注  

 限制控件会对并发调用、实例或会话的数目施加限制以防止过度使用资源。  
  
 每次达到属性值时，就会记录一个跟踪。 第一个跟踪将记录为警告。  
  
## <a name="example"></a>示例  

 下面的配置示例指定服务将最大并发调用数限制为 2，并将最大并发实例数限制为 10。 有关运行此示例的详细示例，请参阅 [限制](../../../wcf/samples/throttling.md)。  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
      <serviceDebug includeExceptionDetailInFaults="False" />
      <serviceMetadata httpGetEnabled="True" />
      <!-- Specify throttling behavior -->
      <serviceThrottling maxConcurrentCalls="2"
                         maxConcurrentInstances="10" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Description.ServiceThrottlingBehavior>
- <xref:System.ServiceModel.Configuration.ServiceThrottlingElement>
- [使用 ServiceThrottlingBehavior 控制 WCF 服务性能](../../../wcf/feature-details/using-servicethrottlingbehavior-to-control-wcf-service-performance.md)
