---
title: 向前兼容的数据协定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], forward compatibility
ms.assetid: 413c9044-26f8-4ecb-968c-18495ea52cd9
ms.openlocfilehash: e8a6cf0cae7519c3ffdbad188c6f67d11a4a6fc1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289797"
---
# <a name="forward-compatible-data-contracts"></a>向前兼容的数据协定

Windows Communication Foundation (WCF) 数据协定系统的一项功能是，协定可以在不间断的情况随时间发展。 也就是说，具有旧版本数据协定的客户端可以与具有相同数据协定的新版本的服务进行通信，或者具有新版本数据协定的客户端可以与相同数据协定的旧版本进行通信。 有关详细信息，请参阅 [最佳做法：数据协定版本控制](../best-practices-data-contract-versioning.md)。  
  
 如果创建了现有数据协定的新版本，您可以根据需要来应用大多数版本管理功能。 但是，必须将一个版本控制功能从第一个版本内置到类型 *中，才能* 正常工作。  
  
## <a name="round-tripping"></a>往返  

 当数据从数据协定的新版本传递到旧版本，然后传递回新版本时，就发生了往返。 往返保证了数据不会丢失。 如果启用往返功能，则可以使类型向前兼容于数据协定版本管理模型所支持的任何未来更改。  
  
 若要对某一特定类型启用往返功能，该类型必须实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口。 该接口包含一个属性，即 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A>（由 <xref:System.Runtime.Serialization.ExtensionDataObject> 类型返回）。 该属性存储数据协定未来版本中对当前版本未知的所有数据。  
  
### <a name="example"></a>示例  

 下面的数据协定不向前兼容于未来的更改。  
  
 [!code-csharp[C_DataContract#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#7)]
 [!code-vb[C_DataContract#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#7)]  
  
 若要使该类型兼容于未来的更改（例如添加名为“phoneNumber”的新数据成员），应实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口。  
  
 [!code-csharp[C_DataContract#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#8)]
 [!code-vb[C_DataContract#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#8)]  
  
 在 WCF 基础结构遇到的数据不是原始数据协定的一部分时，数据存储在属性中并保留。 除非是临时存储，否则不会以任何其他方式处理该数据。 如果对象返回到它的起始位置，则原始（未知）数据也将返回。 由此，数据完成了从起始终结点又回到起始终结点的往返过程而没有丢失。 但请注意，如果起始终结点要求处理数据，则不会满足这一期望，该终结点必须以某种方式检测并调整更改。  
  
 <xref:System.Runtime.Serialization.ExtensionDataObject> 类型不包含公共方法或属性。 因此，无法直接访问存储在 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> 属性内部的数据。  
  
 通过在 `ignoreExtensionDataObject` 构造函数中将 `true` 设置为 <xref:System.Runtime.Serialization.DataContractSerializer>，或者在 <xref:System.ServiceModel.ServiceBehaviorAttribute.IgnoreExtensionDataObject%2A> 中将 `true` 属性设置为 <xref:System.ServiceModel.ServiceBehaviorAttribute>，可以关闭往返功能。 当该功能关闭时，反序列化程序将不填充 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> 属性，序列化程序也不发出该属性的内容。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.IExtensibleDataObject>
- <xref:System.Runtime.Serialization.ExtensionDataObject>
- [数据协定版本管理](data-contract-versioning.md)
- [最佳做法：数据协定版本管理](../best-practices-data-contract-versioning.md)
