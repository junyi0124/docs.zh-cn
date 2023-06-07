---
title: 创建新节点时的 XML 元素和属性名验证
ms.date: 03/30/2017
ms.assetid: b489f647-a175-4659-ada4-170058bb41d0
ms.openlocfilehash: 5bad0faf8aec2f6f1d2395477867fbdaeea4a97a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733056"
---
# <a name="xml-element-and-attribute-name-verification-when-creating-new-nodes"></a>创建新节点时的 XML 元素和属性名验证

XML 文档对象模型 (DOM) 在创建新元素节点或属性节点时检查名称的有效性。 如果名称包含非法字符，则将引发异常。 若要确保名称有效且编码正确，需要使用 XmlConvert  类，在应用一级对名称进行编码和解码。 XmlWriter  包含执行附加操作的方法，以确保生成格式标准的 XML。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
