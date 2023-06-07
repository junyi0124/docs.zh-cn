---
title: MsmqBindingElementBase
ms.date: 03/30/2017
ms.assetid: 210d41ab-a2a4-4d7a-afd2-0916c08a4015
ms.openlocfilehash: 48d26bfa9074fd605e3545579f0bdc2744dfc7d8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267853"
---
# <a name="msmqbindingelementbase"></a>MsmqBindingElementBase

MsmqBindingElementBase  
  
## <a name="syntax"></a>语法  
  
```csharp  
class MsmqBindingElementBase : TransportBindingElement  
{  
  string CustomDeadLetterQueue;  
  string DeadLetterQueue;  
  boolean Durable;  
  boolean ExactlyOnce;  
  sint32 MaxRetryCycles;  
  string ReceiveErrorHandling;  
  sint32 ReceiveRetryCount;  
  datetime RetryCycleDelay;  
  datetime TimeToLive;  
  boolean UseMsmqTracing;  
  boolean UseSourceJournal;  
};  
```  
  
## <a name="methods"></a>方法  

 MsmqBindingElementBase 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 MsmqBindingElementBase 类具有以下属性：  
  
### <a name="customdeadletterqueue"></a>CustomDeadLetterQueue  

 数据类型：字符串  
  
 访问类型：只读  
  
 一个 URI，其中包含每个应用程序的死信队列位置（用于放置已过期的消息或传输/传递失败的消息的位置）。  
  
### <a name="deadletterqueue"></a>DeadLetterQueue  

 数据类型：字符串  
  
 访问类型：只读  
  
 一个枚举值，指示要使用的死信队列的类型。  
  
### <a name="durable"></a>Durable  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个值，指示此绑定处理的消息是持久的还是可变的。  
  
### <a name="exactlyonce"></a>ExactlyOnce  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个 Boolean 值，指示是否只接收过一次此绑定处理的消息。  
  
### <a name="maxretrycycles"></a>MaxRetryCycles  

 数据类型：sint32  
  
 访问类型：只读  
  
 尝试向接收应用程序传递消息的最大重试周期数。  
  
### <a name="receiveerrorhandling"></a>ReceiveErrorHandling  

 数据类型：字符串  
  
 访问类型：只读  
  
 病毒消息处理设置。  
  
### <a name="receiveretrycount"></a>ReceiveRetryCount  

 数据类型：sint32  
  
 访问类型：只读  
  
 从应用程序队列读取消息的最大立即重试次数。  
  
### <a name="retrycycledelay"></a>RetryCycleDelay  

 数据类型：DateTime  
  
 访问类型：只读  
  
 一个值，指示尝试传递无法立即传递的消息时，重试周期之间的时间延迟。  
  
### <a name="timetolive"></a>timeToLive  

 数据类型：DateTime  
  
 访问类型：只读  
  
 时间间隔，指示此绑定所处理的消息在过期之前可以保留在队列中的时间。  
  
### <a name="usemsmqtracing"></a>UseMsmqTracing  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个 Boolean 值，指示是否应跟踪此绑定处理的消息。  
  
### <a name="usesourcejournal"></a>UseSourceJournal  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个 Boolean 值，指示此绑定所处理的消息副本是否应存储在源日志队列中。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.NetMsmqBinding>
- <xref:System.ServiceModel.MsmqBindingBase>
