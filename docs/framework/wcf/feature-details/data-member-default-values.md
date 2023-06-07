---
title: 数据成员默认值
description: 了解如何在数据成员具有 .NET Framework 默认值时从序列化数据中省略该数据成员。 WCF 可以通过不序列化默认值来提高性能。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data members [WCF], default values
- data members [WCF]
ms.assetid: 53a3b505-4b27-444b-b079-0eb84a97cfd8
ms.openlocfilehash: 98b19fdabebeb8a1d43a66a192cb523b82ada618
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261990"
---
# <a name="data-member-default-values"></a>数据成员默认值

在 .NET Framework 中，类型具有 *默认值* 的概念。 例如，对于任何引用类型，默认值为 `null`，而整型的默认值为零。 如果某个数据成员设置为其默认值，有时会希望序列化数据中不包含该数据成员。 由于成员具有默认值，这个实际值不需要进行序列化；这样处理可以提高性能。  
  
 若要在序列化数据中忽略某个成员，请将 <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> 属性 (Attribute) 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (Property) 设置为 `false`（默认值为 `true`）。  
  
> [!NOTE]
> 如果有特定需要（如实现互操作性或减小数据大小），应将 <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> 属性设置为 `false`。  
  
## <a name="example"></a>示例  

 下面的代码中，有几个成员的 <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> 设置为 `false`。  
  
 [!code-csharp[DataMemberAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/datamemberattribute/cs/overview.cs#4)]
 [!code-vb[DataMemberAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datamemberattribute/vb/overview.vb#4)]  
  
 如果序列化此类的实例，则结果如下：序列化 `employeeName` 和 `employeeID`。 `employeeName` 的 null 值和 `employeeID` 的零值显式成为序列化数据的一部分。 但是，`position`、`salary` 和 `bonus` 成员没有被序列化。 最后，即使 `targetSalary` 属性设置为 <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A>，由于 57800 不是 .NET 的整数默认值零，`false` 还是照常被序列化。  
  
### <a name="xml-representation"></a>XML 表示形式  

 如果上一个示例序列化为 XML，则表示形式类似于下面的内容。  
  
```xml  
<Employee>  
   <employeeName xsi:nil="true" />  
   <employeeID>0</employeeID>  
<targetSalary>57800</targetSalary>  
</Employee>  
```  
  
 `xsi:nil` 属性是万维网联合会 (W3C) XML 架构实例命名空间中的一个特殊属性，它提供一种显式表示 null 值的可互操作方式。 请注意，在 XML 中，没有 position、salary 和 bonus 数据成员的任何相关信息。 接收端可将它们分别解释为 `null`、零和 `null`。 无法确保第三方反序列化程序可以进行正确的解释，这就是不建议使用这种模式的原因。 <xref:System.Runtime.Serialization.DataContractSerializer> 类始终为缺少的值选择正确的解释。  
  
### <a name="interaction-with-isrequired"></a>与 IsRequired 交互  

 如 [数据协定版本控制](data-contract-versioning.md)中所述， <xref:System.Runtime.Serialization.DataMemberAttribute> 特性具有一个 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 属性 (默认值为 `false`) 。 该属性 (Property) 指示，当某个给定数据成员进行反序列化时，该数据成员是否必须出现在序列化数据中。 如果 `IsRequired` 设置为 `true`（指示值必须存在）并且 <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> 设置为 `false`（指示如果值设置为其默认值，则该值不得存在），则由于结果相互矛盾，此数据成员的默认值无法序列化。 如果这样的一个数据成员被设置为其默认值（通常为 `null` 或零）并且尝试进行序列化，则会引发 <xref:System.Runtime.Serialization.SerializationException>。  
  
### <a name="schema-representation"></a>架构表示形式  

 `EmitDefaultValue` `false` [数据协定架构参考](data-contract-schema-reference.md)中讨论了在将属性设置为时数据成员的 XML 架构定义语言 (XSD) 架构表示形式的详细信息。 下面是简要概述：  
  
- 当 <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> 设置为时 `false` ，它会在架构中表示为特定于 WINDOWS COMMUNICATION FOUNDATION (WCF) 的注释。 没有用于表示此信息的可交互操作方式。 特别是，架构中的“default”属性不用于此目的，`minOccurs` 属性仅受 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 设置的影响，而 `nillable` 属性仅受数据成员类型的影响。  
  
- 要使用的实际默认值在架构中不存在。 由接收终结点负责对缺少元素进行适当解释。  
  
 对于架构导入， <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> `false` 只要检测到前面提到的 WCF 特定的批注，属性就会自动设置为。 对于属性设置为的引用类型，它还设置为， `false` `nillable` `false` 以支持在使用 ASP.NET Web 服务时经常发生的特定互操作方案。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
