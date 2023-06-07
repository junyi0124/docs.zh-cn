---
title: 版本容错序列化回调
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- OnDeserializedAttribute [WCF]
- OnDeserializingAttribute [WCF]
- OnSerializingAttribute [WCF]
- serialization [WCF], setting default values
- OnSerializedAttribute [WCF]
ms.assetid: aa4a3a6f-05ec-4efd-bdbf-2181e13e6468
ms.openlocfilehash: ad162f24042f30eabee7a1fad2025072b26d9af5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289368"
---
# <a name="version-tolerant-serialization-callbacks"></a>版本容错序列化回调

数据协定编程模型充分支持 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 和 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> 类所支持的版本容错序列化回调方法。  
  
## <a name="version-tolerant-attributes"></a>版本容错属性  

 共有四个回调属性。 每个属性都可应用于序列化/反序列化引擎在不同时间调用的方法。 下表说明使用每个属性的时间。  
  
|Attribute|调用相应方法时。|  
|---------------|---------------------------------------------|  
|<xref:System.Runtime.Serialization.OnSerializingAttribute>|在序列化类型之前调用。|  
|<xref:System.Runtime.Serialization.OnSerializedAttribute>|在序列化类型之后调用。|  
|<xref:System.Runtime.Serialization.OnDeserializingAttribute>|在反序列化类型之前调用。|  
|<xref:System.Runtime.Serialization.OnDeserializedAttribute>|在反序列化类型之后调用。|  
  
 该方法必须接受 <xref:System.Runtime.Serialization.StreamingContext> 参数。  
  
 这些方法主要计划用于版本控制或初始化。 反序列化期间，不调用任何构造函数。 因此，如果传入流中缺少数据成员的数据，则这些成员可能不会正确初始化（为既定的默认值），例如，如果数据来自缺少某些数据成员的类型的上一版本。 若要更正此错误，请使用加有 <xref:System.Runtime.Serialization.OnDeserializingAttribute> 标记的回调方法，如下面的示例中所示。  
  
 只能使用上述每个回调属性对每个类型标记一个方法。  
  
### <a name="example"></a>示例  

 [!code-csharp[C_DataContract#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#9)]
 [!code-vb[C_DataContract#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#9)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.OnSerializingAttribute>
- <xref:System.Runtime.Serialization.OnSerializedAttribute>
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>
- <xref:System.Runtime.Serialization.StreamingContext>
- [版本容错序列化](../../../standard/serialization/version-tolerant-serialization.md)
