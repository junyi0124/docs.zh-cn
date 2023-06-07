---
title: schemeSettings 的 <add> 元素（Uri 设置）
ms.date: 03/30/2017
ms.assetid: 594a7b3b-af23-4cfa-b616-0b2dddb1a705
ms.openlocfilehash: 55fcba41d4dabf8937ebaa11235e9309bcb57952
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91149456"
---
# <a name="add-element-for-schemesettings-uri-settings"></a>schemeSettings 的 \<add> 元素（Uri 设置）

为方案名称添加方案设置。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<uri>**](uri-element-uri-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<schemeSettings>**](schemesettings-element-uri-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**

## <a name="syntax"></a>语法  
  
```xml  
<add
  name="http|https"
  genericUriParserOptions="DontUnescapePathDotsAndSlashes"
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|name|应用此设置的方案名称。 唯一受支持的值为 name = "http" 和 name = "https"。|  
  
## <a name="attribute-name-attribute"></a>{Attribute name}Attribute  
  
|值|描述|  
|-----------|-----------------|  
|genericUriParserOptions|此方案的分析器选项。 唯一受支持的值为 genericUriParserOptions = "DontUnescapePathDotsAndSlashes"。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<schemeSettings> 元素 (Uri 设置) ](schemesettings-element-uri-settings.md)|指定如何分析特定方案的 <xref:System.Uri>。|  
  
## <a name="remarks"></a>备注  

 默认情况下，在 <xref:System.Uri?displayProperty=nameWithType> 执行路径压缩之前，类会取消转义编码的路径分隔符。 这是作为一种安全机制实现的，针对以下攻击：  
  
 `http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 如果将此 URI 向下传递到模块，而不是正确处理百分号编码字符，则可能会导致服务器执行以下命令：  
  
 `c:\Windows\System32\cmd.exe /c dir c:\`  
  
 出于此原因， <xref:System.Uri?displayProperty=nameWithType> 类首先取消转义路径分隔符，然后应用路径压缩。 将上述恶意 URL 传递到 <xref:System.Uri?displayProperty=nameWithType> 类构造函数的结果将生成以下 URI：  
  
 `http://www.microsoft.com/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 使用特定方案的 schemeSettings 配置选项，可以将此默认行为修改为不取消转义百分号编码的路径分隔符。  
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="example"></a>示例  

 下面的示例演示类使用的配置 <xref:System.Uri> ，以支持不转义 http 方案的百分号编码的路径分隔符。  
  
```xml  
<configuration>  
  <uri>  
    <schemeSettings>  
      <add name="http" genericUriParserOptions="DontUnescapePathDotsAndSlashes"/>  
    </schemeSettings>  
  </uri>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Configuration.SchemeSettingElement?displayProperty=nameWithType>
- <xref:System.Configuration.SchemeSettingElementCollection?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection.SchemeSettings%2A?displayProperty=nameWithType>
- <xref:System.GenericUriParserOptions?displayProperty=nameWithType>
- <xref:System.Uri?displayProperty=nameWithType>
- [网络设置架构](index.md)
