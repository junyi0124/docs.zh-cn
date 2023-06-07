---
title: XSLT 扩展对象
ms.date: 03/30/2017
ms.assetid: a4ebdbad-087c-4cfe-acc0-17c48142f81a
ms.openlocfilehash: 0e874bb7679854b75a07eb452e47eba0d30807a5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720784"
---
# <a name="xslt-extension-objects"></a>XSLT 扩展对象

扩展对象用于扩展样式表的功能。 扩展对象通过 <xref:System.Xml.Xsl.XsltArgumentList> 类来维护。  
  
 与使用嵌入脚本相比，使用扩展对象具有以下优点：  
  
- 改善了类的包装和重用。  
  
- 使样式表可以更小而且更容易维护。  
  
 使用 <xref:System.Xml.Xsl.XsltArgumentList> 方法将 XSLT 扩展对象添加到 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 对象。 此时，限定名和命名空间 URI 与扩展对象关联。  
  
> [!NOTE]
> 调用 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 方法要求具有 FullTrust 权限集。 有关详细信息，请参阅[代码访问安全性](../../../framework/misc/code-access-security.md)和[命名权限集](/previous-versions/dotnet/netframework-4.0/4652tyx7(v=vs.100))。  
  
 从扩展对象返回的数据类型是四种 XPath 基本数据类型之一：`number`、`string`、`Boolean` 和 `node set`。  
  
 任何使用 `params` 关键字定义的方法允许传递未指定的参数数目，<xref:System.Xml.Xsl.XslCompiledTransform> 类目前不支持。 利用通过 `params` 关键字定义的方法的 XSLT 样式表也无法正确使用。 有关详细信息，请参阅 [params](../../../csharp/language-reference/keywords/params.md)。  
  
### <a name="to-use-an-xslt-extension-object"></a>使用 XSLT 扩展对象  
  
1. 创建 <xref:System.Xml.Xsl.XsltArgumentList> 对象并使用 <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> 方法添加扩展对象。  
  
2. 从样式表调用扩展对象。  
  
3. 将 <xref:System.Xml.Xsl.XsltArgumentList> 对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法。  
  
## <a name="see-also"></a>请参阅

- [XSLT 转换](xslt-transformations.md)
- [XSLT 安全注意事项](xslt-security-considerations.md)
