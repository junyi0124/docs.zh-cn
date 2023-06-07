---
title: 已声明的 XML 元素和属性的名称
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [XML in Visual Basic]
- element names [XML in Visual Basic]
- names in XML literals [Visual Basic]
- attribute names [XML in Visual Basic]
- XML literals [Visual Basic], element names
ms.assetid: cc110118-b6cf-4ff9-a4e4-6233c90c9fbf
ms.openlocfilehash: 2142674c3de4c5ac9e806c1328daa3efb697beb9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085615"
---
# <a name="names-of-declared-xml-elements-and-attributes-visual-basic"></a>已声明的 XML 元素和特性的名称 (Visual Basic)

本主题提供了在 XML 文本中命名 XML 元素和属性的 Visual Basic 准则。  在 XML 文本中，可以指定本地名称或限定名称。 限定名由 XML 命名空间前缀、冒号和本地名称组成。 有关 XML 命名空间前缀的详细信息，请参阅 [Xml 元素文本](../../../language-reference/xml-literals/xml-element-literal.md)。  
  
## <a name="rules"></a>规则  

 Visual Basic 中的元素或属性的本地名称必须遵循以下规则。  
  
- 它可以从命名空间开始。 它必须以字母字符或下划线开头 (`_`) 。  
  
- 它必须只包含字母字符、十进制数字、下划线、句点 ( ) 和连字符 ( ) 。  
  
- 长度不得超过1024个字符。  
  
- 名称中出现的冒号指示命名空间分界。 因此，您可以使用冒号来指定特定名称的 XML 命名空间。  
  
 此外，还应遵循以下准则。  
  
- XML 1.0 规范保留以字符串 "XML" 开头、大小写不同的所有名称。 因此，请不要将这些名称用于元素和属性名称。  
  
### <a name="name-length-guidelines"></a>名称长度准则  

 在实际情况下，名称应尽可能简短，同时仍能清楚地确定元素的性质。 这可以提高代码的可读性，并减少行长度和源文件大小。  
  
 但是，名称不应太短，因为它不能充分描述元素或代码如何使用它。 这对于代码的可读性非常重要。 如果其他人正在尝试理解它，或者你在写入它后你需要很长的时间，则适当的元素名称可节省时间。  
  
## <a name="case-sensitivity-in-names"></a>名称区分大小写  

 XML 元素名称区分大小写。 这意味着，当 Visual Basic 编译器仅对字母大小写不同的两个名称进行比较时，它会将它们解释为不同的名称。 例如，它将 `ABC` 和 `abc` 引用为单独的元素。  
  
## <a name="xml-namespaces"></a>XML 命名空间  

 创建 XML 元素文本时，可以指定元素名称的 XML 命名空间前缀。 有关详细信息，请参阅 [XML 元素文本](../../../language-reference/xml-literals/xml-element-literal.md)。  
  
## <a name="see-also"></a>请参阅

- [在 Visual Basic 中创建 XML](creating-xml.md)
- [XML 元素文本](../../../language-reference/xml-literals/xml-element-literal.md)
