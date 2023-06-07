---
title: TransportBindingElement
ms.date: 03/30/2017
ms.assetid: 54ecfbee-53c0-410c-a7fa-a98f2e40c545
ms.openlocfilehash: 45bfcd069391156bc85cc4c26f2b172770197a9e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96234838"
---
# <a name="transportbindingelement"></a>TransportBindingElement

TransportBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp
class TransportBindingElement : BindingElement  
{  
  boolean ManualAddressing;  
  sint64 MaxBufferPoolSize;  
  sint64 MaxReceivedMessageSize;  
  string Scheme;  
};  
```  
  
## <a name="methods"></a>方法  

 TransportBindingElement 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 TransportBindingElement 类具有以下属性：  
  
### <a name="manualaddressing"></a>ManualAddressing  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个布尔值，该值指定用户是否要控制消息寻址。  
  
### <a name="maxbufferpoolsize"></a>MaxBufferPoolSize  

 数据类型：sint64  
  
 访问类型：只读  
  
 绑定的最大缓冲池大小。  
  
### <a name="maxreceivedmessagesize"></a>MaxReceivedMessageSize  

 数据类型：sint64  
  
 访问类型：只读  
  
 此绑定所处理的消息的最大大小。  
  
### <a name="scheme"></a>Scheme  

 数据类型：字符串  
  
 访问类型：只读  
  
 传输的 URI 方案。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.TransportBindingElement>
