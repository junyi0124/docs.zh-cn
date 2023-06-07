---
title: ReliableSessionBindingElement
ms.date: 03/30/2017
ms.assetid: effda125-b8d3-4de6-8c0e-f59f5ea8f6eb
ms.openlocfilehash: f91e38ab88cd9f93e9bec0e3a6ca65254bc49570
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273316"
---
# <a name="reliablesessionbindingelement"></a>ReliableSessionBindingElement

ReliableSessionBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp
class ReliableSessionBindingElement : BindingElement  
{  
  datetime AcknowledgementInterval;  
  boolean FlowControlEnabled;  
  datetime InactivityTimeout;  
  sint32 MaxPendingChannels;  
  sint32 MaxRetryCount;  
  sint32 MaxTransferWindowSize;  
  boolean Ordered;  
  integer ReliableMessagingVersion;  
};  
```  
  
## <a name="methods"></a>方法  

 ReliableSessionBindingElement 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 ReliableSessionBindingElement 类具有下列属性：  
  
### <a name="acknowledgementinterval"></a>AcknowledgementInterval  

 数据类型：DateTime  
  
 访问类型：只读  
  
 向工厂创建的可靠通道上的消息源发送确认之前目标等待的时间间隔。  
  
### <a name="flowcontrolenabled"></a>FlowControlEnabled  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个布尔值，指定是否启用流控制。  
  
### <a name="inactivitytimeout"></a>InactivityTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 指定通道出错之前，通道允许其他通信方不发送任何消息的最长持续时间。  
  
### <a name="maxpendingchannels"></a>MaxPendingChannels  

 数据类型：sint32  
  
 访问类型：只读  
  
 侦听器上可等待接受的最大通道数。  
  
### <a name="maxretrycount"></a>MaxRetryCount  

 数据类型：sint32  
  
 访问类型：只读  
  
 可靠通道未收到消息确认时，通过在其基础通道上调用 `Send` 尝试重新传输该消息的最多尝试次数。  
  
### <a name="maxtransferwindowsize"></a>MaxTransferWindowSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 可靠会话的最大传输窗口大小。  
  
### <a name="ordered"></a>已订购  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个布尔值，指定是否保证消息以其发送顺序抵达。  
  
### <a name="reliablemessagingversion"></a>ReliableMessagingVersion  

 数据类型：Integer  
  
 访问类型：只读  
  
 一个整数，指定可靠会话中使用的 WS-ReliableMessaging 协议版本。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.ReliableSessionBindingElement>
