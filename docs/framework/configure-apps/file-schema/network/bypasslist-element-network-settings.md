---
title: <bypasslist> 元素（网络设置）
description: <bypasslist>网络设置元素提供了一组正则表达式，用于描述在 .NET Framework 中不使用代理的地址。
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#bypasslist
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/bypasslist
helpviewer_keywords:
- bypasslist element
- <bypasslist> element
ms.assetid: 124446b7-abb1-4e5e-a492-b64398f268f1
ms.openlocfilehash: 58cdcf046b2a5a292493c5704739b22aa4ec4f17
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178406"
---
# <a name="bypasslist-element-network-settings"></a>\<bypasslist> 元素（网络设置）

提供了一组正则表达式，描述不使用代理的地址。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<defaultProxy>**](defaultproxy-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<bypasslist>**

## <a name="syntax"></a>语法  
  
```xml  
<bypasslist>
</bypasslist>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[add](add-element-for-bypasslist-network-settings.md)|将 IP 地址或 DNS 名称添加到代理跳过列表。|  
|[clear](clear-element-for-bypasslist-network-settings.md)|清除跳过列表。|  
|[remove](remove-element-for-bypasslist-network-settings.md)|从代理跳过列表中删除 IP 地址或 DNS 名称。|  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[defaultProxy](defaultproxy-element-network-settings.md)|配置超文本传输协议 (HTTP) 代理服务器。|  
  
## <a name="remarks"></a>备注  

 绕过列表包含用于描述 <xref:System.Net.WebRequest> 实例直接访问而不是通过代理服务器访问的 uri 的正则表达式。  
  
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
