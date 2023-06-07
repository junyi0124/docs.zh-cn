---
title: XML Descendant Axis Property
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyDescendantsAxis
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML descendant axis property [Visual Basic]
- descendant axis property [Visual Basic]
- XML axis [Visual Basic], descendant
- XML [Visual Basic], accessing
ms.assetid: a178f85b-5d54-438f-8479-40b62af6fe76
ms.openlocfilehash: 52544619171dbc7034baeb5feb61395d81096387
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400248"
---
# <a name="xml-descendant-axis-property-visual-basic"></a>XML 后代轴属性 (Visual Basic)

提供对以下各项的后代的访问： <xref:System.Xml.Linq.XElement> 对象、对象 <xref:System.Xml.Linq.XDocument> 、对象的集合 <xref:System.Xml.Linq.XElement> 或对象的集合 <xref:System.Xml.Linq.XDocument> 。

## <a name="syntax"></a>语法

```vb
object...<descendant>
```

## <a name="parts"></a>组成部分

`object`（必需）。 <xref:System.Xml.Linq.XElement> 对象、<xref:System.Xml.Linq.XDocument> 对象、<xref:System.Xml.Linq.XElement> 对象的集合或 <xref:System.Xml.Linq.XDocument> 对象的集合。

`...<`（必需）。 表示子代轴属性的开头。

`descendant`（必需）。 要访问的子代节点的名称，格式为 [ `prefix:]name` 。

|组成部分|说明|
|----------|-----------------|
|`prefix`|可选。 子代节点的 XML 命名空间前缀。 必须是使用语句定义的全局 XML 命名空间 `Imports` 。|
|`name`|必需。 子代节点的本地名称。 请参阅已[声明的 XML 元素和属性的名称](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)。|

`>`（必需）。 表示子代轴属性的结尾。

## <a name="return-value"></a>返回值

<xref:System.Xml.Linq.XElement> 对象的集合。

## <a name="remarks"></a>备注

您可以使用 XML 子代轴属性，按名称从或对象访问子代节点 <xref:System.Xml.Linq.XElement> <xref:System.Xml.Linq.XDocument> ，或者从或对象的集合访问子代节点 <xref:System.Xml.Linq.XElement> <xref:System.Xml.Linq.XDocument> 。 使用 XML `Value` 属性可访问返回的集合中的第一个子代节点的值。 有关详细信息，请参阅[XML Value 属性](xml-value-property.md)。

Visual Basic 编译器将子代轴属性转换为对方法的调用 <xref:System.Xml.Linq.XContainer.Descendants%2A> 。

## <a name="xml-namespaces"></a>XML 命名空间

子代轴属性中的名称只能使用通过语句全局声明的 XML 命名空间 `Imports` 。 它不能使用在 XML 元素文本中本地声明的 XML 命名空间。 有关详细信息，请参阅[Imports 语句（XML 命名空间）](../statements/imports-statement-xml-namespace.md)。

## <a name="example"></a>示例

下面的示例演示如何从对象中访问名为的第一个子代节点的值 `name` 以及名为的所有子代节点的值 `phone` `contacts` 。

[!code-vb[VbXMLSamples#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#25)]

此代码显示以下文本：

`Name: Patrick Hines`

`Home Phone = 206-555-0144`

## <a name="example"></a>示例

下面的示例声明 `ns` 作为 XML 命名空间前缀。 然后，它使用命名空间的前缀创建 XML 文本，并访问具有限定名称的第一个子节点的值 `ns:name` 。

[!code-vb[VbXMLSamples#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples12.vb#26)]

此代码显示以下文本：

`Name: Patrick Hines`

## <a name="see-also"></a>另请参阅

- <xref:System.Xml.Linq.XElement>
- [XML 轴属性](index.md)
- [XML 文本](../xml-literals/index.md)
- [在 Visual Basic 中创建 XML](../../programming-guide/language-features/xml/creating-xml.md)
- [已声明的 XML 元素和属性的名称](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
