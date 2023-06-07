---
title: BinaryMessageEncodingBindingElement
ms.date: 03/30/2017
ms.assetid: e2bb3cdd-3bbd-4bb5-85fe-570457500a66
ms.openlocfilehash: eb174d12731d7f1bc78f4d709cf043daf2346bd2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96269790"
---
# <a name="binarymessageencodingbindingelement"></a>BinaryMessageEncodingBindingElement

BinaryMessageEncodingBindingElement  
  
## <a name="syntax"></a>语法  
  
```csharp  
class BinaryMessageEncodingBindingElement : MessageEncodingBindingElement  
{  
  sint32 MaxReadPoolSize;  
  sint32 MaxSessionSize;  
  sint32 MaxWritePoolSize;  
  XmlDictionaryReaderQuotas ReaderQuotas;  
};  
```  
  
## <a name="methods"></a>方法  

 BinaryMessageEncodingBindingElement 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 BinaryMessageEncodingBindingElement 类具有下列属性。  
  
## <a name="maxreadpoolsize"></a>MaxReadPoolSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 一个整数，指定在无需分配新读取器的情况下可以同时读取的消息数。  
  
## <a name="maxsessionsize"></a>MaxSessionSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 一个值，指定用于编码的缓冲区的大小（以字节为单位）。  
  
## <a name="maxwritepoolsize"></a>MaxWritePoolSize  

 数据类型：sint32  
  
 访问类型：只读  
  
 一个整数，指定在无需分配新编写器的情况下可以同时发送的消息数。  
  
## <a name="readerquotas"></a>ReaderQuotas  

 数据类型：XmlDictionaryReaderQuotas  
  
 访问类型：只读  
  
 读取器的配额。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>
