---
title: DOM 中的命名空间和 DTD
ms.date: 03/30/2017
ms.assetid: 1e9b55c4-76ad-4f54-8d96-7ce4b4cf1e05
ms.openlocfilehash: 42bd30e7eea2ec0a3e1aa6846196c3280697efdf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714544"
---
# <a name="namespaces-and-dtds-in-the-dom"></a>DOM 中的命名空间和 DTD

文档类型定义 (DTD) 使命名空间支持变得很复杂。 例如，在下面的 XML 中，默认属性的名称中包含冒号。  
  
```xml  
<!ATTLIST item x:id CDATA #IMPLIED>  
```  
  
 如果允许使用此构造，则可采用下列解析：  
  
- `x:` 被视为命名空间前缀，但此前缀必须可以使用 `xmlns:x` 命名空间声明来解析，而该声明必须也存在于 DTD 中的某个位置。 将此前缀映射到实例文档中的其他内容是错误的。  
  
- `x:` 被视为命名空间前缀，但此前缀总是在实例元素的上下文中解析。 这意味着根据出现 `item` 元素的命名空间范围，此前缀实际上可映射到其他命名空间统一资源标识符 (URI)。 此行为比前一个项目符号中给定的解析更容易预知，但存在其他复杂的后果，这是因为它要求将默认属性具体化。  
  
- 忽略冒号，因为它在 DTD 中，且属性名为 `x:y`，没有前缀，也没有命名空间 URI。  
  
- 默认属性中的冒号引发异常，表明 DTD 内部不支持在名称中使用冒号。 这导致可预知的行为，但意味着不能加载多个万维网联合会 (W3C) 已发布的 DTD。  
  
- 当用户请求 DTD 验证时，对整个文档的命名空间支持将被关闭。 这使得可以加载 W3C DTD 并产生可预知的行为。  
  
 Microsoft .NET Framework 中的 XML 实现第二个选项，以最大限度地提升 W3C 兼容性。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
