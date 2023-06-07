---
title: XML 值属性
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyExtensionValue
helpviewer_keywords:
- Value property [Visual Basic]
- Visual Basic code, accessing XML
- XML axis [Visual Basic], Value
- XML Value property [Visual Basic]
ms.assetid: 7ddd057a-a195-4e9b-ad8b-2ee0e615a20f
ms.openlocfilehash: 571d9130ef69df580bbba5d90bc8758b4b627196
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349424"
---
# <a name="xml-value-property-visual-basic"></a>XML 值属性 (Visual Basic)

提供对 <xref:System.Xml.Linq.XElement> 对象的集合中第一个元素的值的访问。

## <a name="syntax"></a>语法

```vb
object.Value
```

## <a name="parts"></a>部件

|术语|Definition|  
|---|---|  
|`object`|必需。 <xref:System.Xml.Linq.XElement> 对象的集合。|  

## <a name="return-value"></a>返回值

 一个 `String`，其中包含集合中第一个元素的值，如果集合为空，则为 `Nothing`。

## <a name="remarks"></a>备注

 利用 <xref:System.Xml.Linq.XElement.Value%2A> 属性，可轻松访问 <xref:System.Xml.Linq.XElement> 对象集合中第一个元素的值。 此属性首先检查集合是否包含至少一个对象。 如果集合为空，此属性将返回 `Nothing`。 否则，此属性将返回集合中第一个元素的 <xref:System.Xml.Linq.XElement.Value%2A> 属性的值。

> [!NOTE]
> 使用 "\@" 标识符访问 XML 特性的值时，特性值作为 `String` 返回，无需显式指定 <xref:System.Xml.Linq.XAttribute.Value%2A> 属性。

 若要访问集合中的其他元素，可以使用 XML 扩展索引器属性。 有关详细信息，请参阅[扩展索引器属性](extension-indexer-property.md)。

## <a name="inheritance"></a>继承

 大多数用户不需要实现 <xref:System.Collections.Generic.IEnumerable%601>，因此可以忽略此部分。

 <xref:System.Xml.Linq.XElement.Value%2A> 属性是实现 `IEnumerable(Of XElement)`的类型的扩展属性。 此扩展属性的绑定类似于扩展方法的绑定：如果某个类型实现了一个接口，并定义了一个名称为 "Value" 的属性，则该属性的优先级高于扩展属性。 换言之，可以通过在实现 `IEnumerable(Of XElement)`的类中定义一个新属性，来重写此 <xref:System.Xml.Linq.XElement.Value%2A> 属性。

## <a name="example"></a>示例

 下面的示例演示如何使用 <xref:System.Xml.Linq.XElement.Value%2A> 属性访问 <xref:System.Xml.Linq.XElement> 对象的集合中的第一个节点。 该示例使用子轴属性来获取 `contact` 对象中名为 `phone` 的所有子节点的集合。

 [!code-vb[VbXMLSamples#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples7.vb#15)]

 此代码显示以下文本：

 `Phone number: 206-555-0144`

## <a name="example"></a>示例

 下面的示例演示如何从 <xref:System.Xml.Linq.XAttribute> 对象的集合获取 XML 特性的值。 该示例使用 "属性轴" 属性显示所有 `phone` 元素的 `type` 属性的值。

 [!code-vb[VbXMLSamples#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples7.vb#16)]

 此代码显示以下文本：

 ```console
 home
 work
```

## <a name="see-also"></a>另请参阅

- <xref:System.Xml.Linq.XElement>
- <xref:System.Collections.Generic.IEnumerable%601>
- [XML 轴属性](index.md)
- [XML 文本](../xml-literals/index.md)
- [在 Visual Basic 中创建 XML](../../programming-guide/language-features/xml/creating-xml.md)
- [扩展方法](../../programming-guide/language-features/procedures/extension-methods.md)
- [扩展索引器属性](extension-indexer-property.md)
- [XML 子轴属性](xml-child-axis-property.md)
- [XML 特性轴属性](xml-attribute-axis-property.md)
