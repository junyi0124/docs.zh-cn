---
title: <uri> 元素（Uri 设置）
ms.date: 03/30/2017
ms.assetid: c22bab8b-477c-4ae4-8498-65ad409e0847
ms.openlocfilehash: 2f22d70407d10dbb38f0cb8d3a8ac74ff3fe8763
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91190197"
---
# <a name="uri-element-uri-settings"></a>\<uri> 元素（Uri 设置）

包含指定 .NET Framework 如何处理使用统一资源标识符 (Uri) 表示的 web 地址的设置。  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<uri>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<uri>  
</uri>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[idn](idn-element-uri-settings.md)|指定是否对域名应用国际化域名 (IDN) 分析。|  
|[Iriparsing>](iriparsing-element-uri-settings.md)|指定是否将国际化资源标识符 (IRI) 分析应用到 <xref:System.Uri> ，以及是否应该应用 iri 分析规则。|  
|[schemeSettings](schemesettings-element-uri-settings.md)|指定如何分析特定方案的 <xref:System.Uri>。|  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[configuration](../configuration-element.md)|包含所有命名空间的设置。|  
  
## <a name="remarks"></a>备注  

 `uri`元素包含 <xref:System.Uri> 由命名空间中的类使用的类的成员设置 <xref:System.Net> 。 设置配置对 IRI 和 IDN 的支持。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例演示类使用的配置， <xref:System.Uri> 以支持 IRI 分析和 IDN 名称。 该示例还会清除所有方案设置，然后添加对 http 方案不转义百分号编码路径分隔符的支持。  
  
### <a name="code"></a>代码  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All" />  
    <iriParsing enabled="true" />  
    <schemeSettings>  
      <clear/>  
      <add name="http" genericUriParserOptions="DontUnescapePathDotsAndSlashes"/>  
    </schemeSettings>  
  </uri>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [网络设置架构](index.md)
