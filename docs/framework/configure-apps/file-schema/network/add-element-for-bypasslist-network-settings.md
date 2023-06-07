---
title: bypasslist 的 <add> 元素（网络设置）
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/bypasslist/add
- http://schemas.microsoft.com/.NetConfiguration/v2.0#add
helpviewer_keywords:
- <bypasslist>, add element
- bypasslist, add element
- <add> element, bypasslist
- add element, bypasslist
ms.assetid: a0b86e28-86b4-4497-abe8-d5fd614c7926
ms.openlocfilehash: 927a43f352fd776d9e6ba52ebea6ba2a1ccd4d48
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91149486"
---
# <a name="add-element-for-bypasslist-network-settings"></a>bypasslist 的 \<add> 元素（网络设置）

将 IP 地址或 DNS 名称添加到代理跳过列表。  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;[**\<defaultProxy>**](defaultproxy-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<bypasslist>**](bypasslist-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<add
  address="regular expression"
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|**Attribute**|**说明**|  
|-------------------|---------------------|  
|**address**|描述 IP 地址或 DNS 名称的正则表达式。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[bypasslist](bypasslist-element-network-settings.md)|提供了一组正则表达式，描述不使用代理的地址。|  
  
## <a name="remarks"></a>备注  

 `add`元素将描述 IP 地址或 DNS 服务器名称的正则表达式插入绕过代理服务器的地址列表。  
  
 特性的值 `address` 应为描述一组 IP 地址或主机名的正则表达式。  
  
 为此元素指定正则表达式时，应格外小心。 正则表达式 "[a-z] + \\ \\ .com" 与 contoso.com 域中的任何主机匹配，但它还匹配 contoso.com.cpandl.com 域中的任何主机。 若要仅匹配 contoso.com 域中的主机，请使用锚 ( "$" ) ： "[a-z] + \\ \\ .com $"。  
  
 有关正则表达式的详细信息，请参阅。[.NET Framework 正则表达式](../../../../standard/base-types/regular-expressions.md)。  
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="example"></a>示例  

 下面的示例将两个地址添加到跳过列表。 首先，将跳过 contoso.com 域中所有服务器的代理;第二种方式是跳过其 IP 地址以192.168 开头的所有服务器的代理。  
  
```xml  
<configuration>  
  <system.net>  
    <defaultProxy>  
      <bypasslist>  
        <add address="[a-z]+\.contoso\.com$" />  
        <add address="192\.168\.\d{1,3}\.\d{1,3}" />  
      </bypasslist>  
    </defaultProxy>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [网络设置架构](index.md)
