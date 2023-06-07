---
title: 推断简单类型的规则
ms.date: 03/30/2017
ms.assetid: 394624d6-4da0-430a-8a88-46efe40f14de
ms.openlocfilehash: b8fa3037d9ad5af057f477733ffdea74681f5549
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686535"
---
# <a name="rules-for-inferring-simple-types"></a>推断简单类型的规则

描述 <xref:System.Xml.Schema.XmlSchemaInference> 类如何推断属性和元素的数据类型。  
  
 <xref:System.Xml.Schema.XmlSchemaInference> 类将属性和元素的数据类型推断为简单类型。 本节描述可能的推断类型、如何将多个不同的值协调为单个类型以及如何处理架构定义 `xsi` 属性。  
  
## <a name="inferred-types"></a>推断类型  

 <xref:System.Xml.Schema.XmlSchemaInference> 类将元素和属性值推断为简单类型，并在生成的架构中包含类型属性。 所有推断类型都是简单类型。 生成的架构中不包括任何基类型或方面。  
  
 在 XML 文档中遇到的值会分别进行检查。 在检查时会推断值的类型。 如果已推断了某个属性或元素的类型，在遇到该属性或元素的值与当前的推断类型不匹配时，<xref:System.Xml.Schema.XmlSchemaInference> 类将提升规则集中每个规则的类型。 这些规则在本主题后面的“类型提升”一节中讨论。  
  
 下表列出生成的架构中可能包含的推断类型。  
  
|简单类型|描述|  
|-----------------|-----------------|  
|boolean|true、false、0、1.|  
|byte|范围在 –128 到 127 之间的整数。|  
|unsignedByte|范围在 0 到 255 之间的整数。|  
|short|范围在 -32768 到 32767 之间的整数。|  
|unsignedShort|范围在 0 到 65535 之间的整数。|  
|int|范围在 -2147483648 到 2147483647 之间的整数。|  
|unsignedInt|范围在 0 到 4294967295 之间的整数。|  
|long|范围在 -9223372036854775808 到 9223372036854775807 之间的整数。|  
|unsignedLong|范围在 0 到 18446744073709551615 之间的整数。|  
|integer|可能使用“-”前缀的有穷位数字。|  
|decimal|精度为 0 到 28 位的数值。|  
|float|可以依次后接“E”或“e”和表示指数的整数值的十进制数。 十进制值的范围可以在 -16777216 到 16777216 之间。 指数值的范围可以在 -149 到 104 之间。<br /><br /> float 允许表示无穷值和非数值的特殊值。 特殊的浮点型值有：0、-0、INF、-INF、NaN。|  
|double|与 float 相同，只是十进制值的范围可以在 -9007199254740992 到 9007199254740992 之间，指数值的范围可以在 –1075 到 970 之间。<br /><br /> double 允许表示无穷值和非数值的特殊值。 特殊的浮点型值有：0、-0、INF、-INF、NaN。|  
|duration|W3C duration 格式。|  
|dateTime|W3C dateTime 格式。|  
|time|W3C time 格式。|  
|date|年份值限制在 0001 到 9999 之间。|  
|gYearMonth|W3C 公历月份和年份的格式。|  
|string|一个或多个 Unicode 字符。|  
  
## <a name="type-promotion"></a>类型提升  

 <xref:System.Xml.Schema.XmlSchemaInference> 类一次检查一个属性和元素的值。 在遇到值时，将推断限制性最强的无符号类型。 如果已推断了某个属性或元素的类型，在遇到与当前推断类型不匹配的新值时，推断类型将提升为新类型，以便适用于当前推断类型和新值。 <xref:System.Xml.Schema.XmlSchemaInference> 类在提升推断类型时会考虑以前的值。  
  
 例如，考虑两个 XML 文档中的以下 XML 片断：  
  
 `<MyElement1 attr1="12" />`  
  
 `<MyElement1 attr1="52344" />`  
  
 在遇到第一个 `attr1` 值时，根据值 `attr1`，`unsignedByte` 的类型推断为 `12`。 在遇到第二个 `attr1` 时，根据当前推断类型 `unsignedShort` 和当前值 `unsignedByte`，类型提升为 `52344`。  
  
 现在，考虑两个 XML 文档中的以下 XML：  
  
 `<MyElement2 attr2="0" />`  
  
 `<MyElement2 attr2="true" />`  
  
 在遇到第一个 `attr2` 值时，根据值 `attr2`，`unsignedByte` 的类型推断为 `0`。 在遇到第二个 `attr2` 时，根据当前推断类型 `string` 和当前值 `unsignedByte`，类型提升为 `true`，因为 <xref:System.Xml.Schema.XmlSchemaInference> 类在提升推断类型时会考虑以前的值。 但是，如果在同一个 XML 文档中遇到 `attr2` 的两个实例，而不是如上所述在两个不同的 XML 文档中，`attr2` 将推断为 `boolean`。  
  
### <a name="ignored-attributes-from-the-httpswwww3org2001xmlschema-instance-namespace"></a>从 <https://www.w3.org/2001/XMLSchema-instance> 命名空间忽略的属性

在架构引用过程中忽略下列架构定义属性。  
  
|特性|描述|  
|---------------|-----------------|  
|`xsi:type`|如果遇到指定了 `xsi:type` 类型的元素，将忽略 `xsi:type`。|  
|`xsi:nil`|如果遇到具有 `xsi:nil` 属性的元素，推断架构中的元素声明的值将为 `nillable="true"`。 `xsi:nil` 属性设置为 `true` 的元素不能包含子元素。|  
|`xsi:schemaLocation`|如果遇到 `xsi:schemaLocation`，将忽略。|  
|`xsi:noNamespaceSchemaLocation`|如果遇到 `xsi:noNamespaceSchemaLocation`，将忽略。|  
  
## <a name="see-also"></a>请参阅

- [XML 架构对象模型 (SOM)](xml-schema-object-model-som.md)
- [从 XML 文档推断架构](inferring-schemas-from-xml-documents.md)
- [推断架构节点类型和结构的规则](rules-for-inferring-schema-node-types-and-structure.md)
