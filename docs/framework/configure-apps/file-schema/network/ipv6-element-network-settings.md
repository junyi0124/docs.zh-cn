---
title: <ipv6> 元素（网络设置）
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings/ipv6
- http://schemas.microsoft.com/.NetConfiguration/v2.0#ipv6
helpviewer_keywords:
- <ipv6> element
- ipv6 element
ms.assetid: 10b79aef-327b-4718-a892-e11f55e4d169
ms.openlocfilehash: 44ef0b8e1b6dc6ad0efde6b26ad7d4700e06f2c7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178328"
---
# <a name="ipv6-element-network-settings"></a>\<ipv6> 元素（网络设置）

使 Internet 协议版本 6 (IPv6) 来自类的过时成员的响应 <xref:System.Net.Dns> 。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<settings>**](settings-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<ipv6>**

## <a name="syntax"></a>语法  
  
```xml  
<ipv6  
  enabled="true|false"  
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|**Attribute**|**说明**|  
|-------------------|---------------------|  
|`enabled`|指定类的成员是否 <xref:System.Net.Dns> 返回 Internet 协议版本 6 (IPv6) 地址。 默认值是 `false`。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[设置](settings-element-network-settings.md)|配置 <xref:System.Net> 命名空间的基本网络选项。|  
  
## <a name="remarks"></a>备注  

 此设置为类的过时成员启用 IPv6 支持 <xref:System.Net.Dns> ： <xref:System.Net.Dns.BeginGetHostByName%2A> 、 <xref:System.Net.Dns.BeginResolve%2A> 、 <xref:System.Net.Dns.EndGetHostByName%2A> 、、、 <xref:System.Net.Dns.EndResolve%2A> <xref:System.Net.Dns.GetHostByAddress%2A> <xref:System.Net.Dns.GetHostByName%2A> 和 <xref:System.Net.Dns.Resolve%2A> 。 对于命名空间的其他成员 <xref:System.Net?displayProperty=nameWithType> ，如果在操作系统中启用了 ipv6，则可能会返回 ipv6 地址。  
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="example"></a>示例  

 下面的示例演示如何为类启用 IPv6 支持 <xref:System.Net.Dns> 。  
  
```xml  
<configuration>  
  <system.net>  
    <settings>  
      <ipv6 enabled="true"/>  
    </settings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net?displayProperty=nameWithType>
- <xref:System.Net.Dns?displayProperty=nameWithType>
- <xref:System.Net.Sockets.Socket.OSSupportsIPv6%2A?displayProperty=nameWithType>
- [网络设置架构](index.md)
