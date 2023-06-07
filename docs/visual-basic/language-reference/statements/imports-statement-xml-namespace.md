---
title: Imports 语句-XML 命名空间
ms.date: 07/20/2015
f1_keywords:
- vb.ImportsXmlns
helpviewer_keywords:
- XML namespace [Visual Basic], importing
- imports [Visual Basic]
- Imports statement [Visual Basic]
- namespaces [Visual Basic], importing
ms.assetid: 1f4d50a6-08c7-4c2e-8206-ccae35fcd1b4
ms.openlocfilehash: a3184d68b0e4cdff5d4296a5a638e22b4e83bcde
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404532"
---
# <a name="imports-statement-xml-namespace"></a>Imports 语句（XML 命名空间）

导入用于 XML 文本和 XML 轴属性的 XML 命名空间前缀。

## <a name="syntax"></a>语法

```vb
Imports <xmlns:xmlNamespacePrefix = "xmlNamespaceName">
```

## <a name="parts"></a>组成部分

`xmlNamespacePrefix`  
可选。 XML 元素和特性可以引用的字符串 `xmlNamespaceName` 。 如果未 `xmlNamespacePrefix` 提供，则导入的 xml 命名空间为默认的 xml 命名空间。 必须是有效的 XML 标识符。 有关详细信息，请参阅已[声明的 XML 元素和属性的名称](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)。

`xmlNamespaceName`  
必需。 标识要导入的 XML 命名空间的字符串。

## <a name="remarks"></a>备注

可以使用 `Imports` 语句来定义可用于 xml 文本和 xml 轴属性的全局 XML 命名空间，或用作传递给运算符的参数 `GetXmlNamespace` 。 （有关使用 `Imports` 语句导入可用于在代码中使用类型名称的别名的信息，请参阅[Imports 语句（.Net 命名空间和类型）](imports-statement-net-namespace-and-type.md)。）使用语句声明 XML 命名空间的语法与 `Imports` XML 中使用的语法相同。 因此，您可以复制 XML 文件中的命名空间声明，并在语句中使用它 `Imports` 。

如果要重复创建来自相同命名空间的 XML 元素，XML 命名空间前缀会很有用。 使用语句声明的 XML 命名空间前缀 `Imports` 是全局性的，因为它可用于文件中的所有代码。 当你创建 XML 元素文本以及访问 XML 轴属性时，可以使用此方法。 有关详细信息，请参阅[XML 元素文本](../xml-literals/xml-element-literal.md)和[xml 轴属性](../xml-axis/index.md)。

如果定义不带命名空间前缀的全局 XML 命名空间（例如 `Imports <xmlns="http://SomeNameSpace>"` ），则该命名空间将被视为默认的 xml 命名空间。 默认的 XML 命名空间用于未显式指定命名空间的任何 XML 元素文本或 XML 特性轴属性。 如果指定的命名空间为空命名空间，则也会使用默认命名空间（即 `xmlns=""` ）。 默认的 XML 命名空间不适用于 XML 文本中的 XML 特性或没有命名空间的 XML 特性轴属性。

在 XML 文本中定义的 XML 命名空间称为*本地 xml 命名空间*，其优先级高于作为全局语句定义的 xml 命名空间 `Imports` 。 语句定义的 XML 命名空间 `Imports` 优先于为 Visual Basic 项目导入的 xml 命名空间。 如果 XML 文本定义了 XML 命名空间，则该本地命名空间不适用于嵌入的表达式。

全局 XML 命名空间遵循与 .NET Framework 命名空间相同的作用域和定义规则。 因此，可以在 `Imports` 可导入 .NET Framework 命名空间的任何位置包含语句来定义全局 XML 命名空间。 这包括代码文件和项目级别导入的命名空间。 有关项目级别导入命名空间的信息，请参阅[引用页、项目设计器（Visual Basic）](/visualstudio/ide/reference/references-page-project-designer-visual-basic)。

每个源文件可以包含任意多个 `Imports` 语句。 它们必须遵循选项声明，如 `Option Strict` 语句，它们必须位于编程元素声明（如或语句）之前 `Module` `Class` 。

## <a name="example"></a>示例

下面的示例导入使用前缀标识的默认 XML 命名空间和 XML 命名空间 `ns` 。 然后创建使用这两个命名空间的 XML 文本。

[!code-vb[VbXMLSamples#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/Module1.vb#45)]

此代码显示以下文本：

```xml
<ns:outer xmlns="http://DefaultNamespace"
          xmlns:ns="http://NewNamespace">
  <ns:innerElement></ns:innerElement>
  <siblingElement></siblingElement>
  <innerElement />
</ns:outer>
```

## <a name="example"></a>示例

下面的示例导入 XML 命名空间前缀 `ns` 。 然后，它创建一个使用命名空间前缀的 XML 文本，并显示该元素的最终形式。

[!code-vb[VbXMLSamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples10.vb#22)]

此代码显示以下文本：

```xml
<ns:outer xmlns:ns="http://SomeNamespace">
  <ns:middle xmlns:ns="http://NewNamespace">
    <ns:inner1 />
    <inner2 xmlns="http://SomeNamespace" />
  </ns:middle>
</ns:outer>
```

请注意，编译器将 XML 命名空间前缀从全局前缀转换为本地前缀定义。

## <a name="example"></a>示例

下面的示例导入 XML 命名空间前缀 `ns` 。 然后它使用该命名空间前缀来创建 XML 文本并访问具有限定名称 `ns:name` 的第一个子节点。

[!code-vb[VbXMLSamples#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples8.vb#19)]

此代码显示以下文本：

`Patrick Hines`

## <a name="see-also"></a>另请参阅

- [XML 元素文本](../xml-literals/xml-element-literal.md)
- [XML 轴属性](../xml-axis/index.md)
- [已声明的 XML 元素和属性的名称](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
- [GetXmlNamespace 运算符](../operators/getxmlnamespace-operator.md)
