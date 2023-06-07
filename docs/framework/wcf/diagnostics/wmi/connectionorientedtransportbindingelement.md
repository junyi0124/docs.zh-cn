---
title: ConnectionOrientedTransportBindingElement
ms.date: 03/30/2017
ms.assetid: c1308313-f0e2-49e6-977d-6b4ce9ad35d1
ms.openlocfilehash: 3c69b73cc05a56a7556630de0f83675590442293
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274148"
---
# <a name="connectionorientedtransportbindingelement"></a>ConnectionOrientedTransportBindingElement

ConnectionOrientedTransportBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp
class ConnectionOrientedTransportBindingElement : TransportBindingElement  
{  
  datetime ChannelInitializationTimeout;  
  sint32 ConnectionBufferSize;  
  string HostNameComparisonMode;  
  sint32 MaxBufferSize;  
  datetime MaxOutputDelay;  
  sint32 MaxPendingAccepts;  
  sint32 MaxPendingConnections;  
  string TransferMode;  
};  
```  
  
## <a name="methods"></a>方法  

 ConnectionOrientedTransportBindingElement 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 ConnectionOrientedTransportBindingElement 类具有以下属性：  
  
### <a name="channelinitializationtimeout"></a>ChannelInitializationTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 指定在超时前可用于完成通道初始化的时间间隔。  
  
### <a name="connectionbuffersize"></a>ConnectionBufferSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 用于从客户端或服务传输网络上的序列化消息块的缓冲区大小。  
  
### <a name="hostnamecomparisonmode"></a>HostNameComparisonMode  

 数据类型：字符串  
  
 访问类型：只读  
  
 一个值，指示在对 URI 进行匹配时，是否使用主机名来访问服务。  
  
### <a name="maxbuffersize"></a>MaxBufferSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 要使用的缓冲区最大大小。  
  
### <a name="maxoutputdelay"></a>MaxOutputDelay  

 数据类型：DateTime  
  
 访问类型：只读  
  
 消息块或完整消息在发出之前可以在内存中保持缓冲的最大时间间隔。  
  
### <a name="maxpendingaccepts"></a>MaxPendingAccepts  

 数据类型：sint32  
  
 访问类型：只读  
  
 可用于处理服务上的传入连接的最大挂起异步接受线程数。  
  
### <a name="maxpendingconnections"></a>MaxPendingConnections  

 数据类型：sint32  
  
 访问类型：只读  
  
 最大挂起连接数。  
  
### <a name="transfermode"></a>TransferMode  

 数据类型：字符串  
  
 访问类型：只读  
  
 一个值，指定通过面向连接的传输对消息进行缓冲还是流处理。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement>
