---
title: 在 XSLT 处理期间解析外部资源
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3a59d31c-0ec5-4de6-a2a9-558531c8116e
ms.openlocfilehash: 5d50711eda266cecdb817c778f04aa845fa4c342
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686639"
---
# <a name="resolving-external-resources-during-xslt-processing"></a>在 XSLT 处理期间解析外部资源

XSLT 转换过程中会有几个场合需要解析外部资源。  
  
## <a name="using-the-xmlresolver-class"></a>使用 XmlResolver 类  

 <xref:System.Xml.XmlResolver> 类用于解析外部资源。 下表说明在 XSLT 处理期间 <xref:System.Xml.XmlResolver> 参与的时间。  
  
|XSLT 任务|XmlResolver 的用途|  
|---------------|--------------------------------------|  
|编译样式表。|解析样式表的 URI。<br /><br /> －和－<br /><br /> 解析任何 `xsl:import` 或 `xsl:include` 元素中的 URI 引用。|  
|执行样式表。|解析上下文文档的 URI。<br /><br /> －和－<br /><br /> 解析任何 XSLT `document()` 函数中的 URI 引用。|  
  
 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 和 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法包括使用 <xref:System.Xml.XmlResolver> 对象作为一个参数的重载。 如果未指定 <xref:System.Xml.XmlResolver>，将使用没有用户凭据的默认 <xref:System.Xml.XmlUrlResolver>。  
  
 下表说明可能需要指定 <xref:System.Xml.XmlResolver> 对象的情况：  
  
- 如果 XSLT 进程需要访问要求进行身份验证的网络资源，可以使用具有必要的凭据的 <xref:System.Xml.XmlResolver>。  
  
- 如果要限制 XSLT 进程可以访问的资源，可以使用具有正确权限集的 <xref:System.Xml.XmlSecureResolver>。 如果需要打开自己无法控制的或不可信的资源，请使用 <xref:System.Xml.XmlSecureResolver> 类。  
  
- 如果要自定义行为，可以实现自己的 <xref:System.Xml.XmlResolver> 类并使用该类解析资源。  
  
- 如果要确保不访问任何外部资源，可以为 `null` 自变量指定 <xref:System.Xml.XmlResolver>。  
  
## <a name="example"></a>示例  

 以下示例编译存储在网络资源上的样式表。 <xref:System.Xml.XmlUrlResolver> 对象指定访问该样式表所需的凭据。  
  
 [!code-csharp[XslCompiledTransform.Load#11](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Load/CS/Xslt_Load_v2.cs#11)]
 [!code-vb[XslCompiledTransform.Load#11](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Load/VB/Xslt_Load_v2.vb#11)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Xsl.XslCompiledTransform>
- <xref:System.Xml.Xsl.XsltSettings>
- [XSLT 转换](xslt-transformations.md)
