---
title: DOM 中的命名空间支持
ms.date: 03/30/2017
ms.assetid: f0548ead-0fed-41ee-b33e-117ba900d3bc
ms.openlocfilehash: b3214d77b069b672e8772ec78db51c9d8ee1bf50
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714401"
---
# <a name="namespace-support-in-the-dom"></a>DOM 中的命名空间支持

XML 文档对象模型 (DOM) 完全识别命名空间。 只支持识别命名空间的 XML 文档。 万维网联合会 (W3C) 指定实现级别 1 的 DOM 应用程序可以不识别命名空间，而 DOM 级别 2 功能识别命名空间。 然而，XML DOM 中的所有功能都识别命名空间，不论该方法来自级别 1 还是级别 2 DOM 建议。  
  
 例如，在不识别命名空间的设置中，调用 DOM 级别 1 建议中指定的 `setAttribute("A:b", "123")` 不会生成前缀为 `A`、本地名称为 `b` 的属性。 将产生值为 `A:b` 的属性。  
  
 在识别命名空间的环境中，调用 DOM 级别 2 `setAttribute("A:b", "123")` 将生成前缀为 `A`、本地名称为 `b` 的属性。 这就是 Microsoft .NET Framework DOM 的工作原理。  
  
 因此，对于所有接受名称参数的方法，这些方法也接受用于限定名称的前缀。 名称参数（如 setAttribute  DOM 级别 1 方法中的 `A:b`）按如下方式进行分析：  
  
- 如果没有冒号 (:) 字符，则本地名称设置为 `name` 参数，而前缀和 NamespaceURI 为空字符串。  
  
- 如果找到冒号，则根据第一个冒号字符的位置将该名称分为两个部分。 将前缀设置为该冒号前面的字符串，将本地名称设置为该冒号后面的字符串。 对于不接受 NamespaceURI 值的方法，不解析 NamespaceURI 并保持设置为空字符串。 否则，将 NamespaceURI 设置为传递给该方法的字符串。 如果未定义前缀，Save  方法以及 InnerXml  和 OuterXml  属性失败。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
