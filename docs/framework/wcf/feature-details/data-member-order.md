---
title: 数据成员顺序
description: 了解 WCF 中的数据成员顺序。 应用程序可能需要知道或更改数据成员发送或预期的顺序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], ordering members
ms.assetid: 0658a47d-b6e5-4ae0-ba72-ababc3c6ff33
ms.openlocfilehash: 1cb63569c1789b1577588caf63fb0a0259e530ff
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262159"
---
# <a name="data-member-order"></a>数据成员顺序

在一些应用程序中，有必要知道各个数据成员中数据的发送顺序或预期接收顺序（比如序列化 XML 中数据的显示顺序）。 有时，必须要更改此顺序。 本主题说明排序规则。  
  
## <a name="basic-rules"></a>基本规则  

 数据排序的基本规则包括：  
  
- 如果数据协定类型是继承层次结构的一部分，则其基类型的数据成员始终排在第一位。  
  
- 排在下一位的是当前类型的数据成员（按字母顺序排列），这些成员未设置 <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> 属性 (attribute) 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (property)。  
  
- 再下面是设置了 <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> 属性 (attribute) 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (property) 的任何数据成员。 这些成员首先按 `Order` 属性的值排序，如果多个成员具有特定的 `Order` 值，则按字母顺序排列。 可以跳过 Order 值。  
  
 字母顺序是通过调用 <xref:System.String.CompareOrdinal%2A> 方法确定的。  
  
## <a name="examples"></a>示例  

 考虑下列代码。  
  
 [!code-csharp[C_DataContractNames#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#4)]
 [!code-vb[C_DataContractNames#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#4)]  
  
 生成的 XML 类似于以下形式。  
  
```xml  
<DerivedType>  
    <!-- Zebra is a base data member, and appears first. -->  
    <zebra/>
  
    <!-- Cat has no Order, appears alphabetically first. -->  
    <cat/>  
  
   <!-- Dog has no Order, appears alphabetically last. -->  
    <dog/>
  
    <!-- Bird is the member with the smallest Order value -->  
    <bird/>  
  
    <!-- Albatross has the next Order value, alphabetically first. -->  
    <albatross/>  
  
    <!-- Parrot, with the next Order value, alphabetically last. -->  
     <parrot/>  
  
    <!-- Antelope is the member with the highest Order value. Note that   
    Order=2 is skipped -->  
     <antelope/>
</DerivedType>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataContractAttribute>
- [数据协定等效性](data-contract-equivalence.md)
- [使用数据协定](using-data-contracts.md)
