---
title: <defaultProxy> 元素（网络设置）
description: <defaultProxy>网络设置元素在 .NET Framework 中 (HTTP) 代理服务器配置超文本传输协议。
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#defaultProxy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy
helpviewer_keywords:
- defaultProxy element
- <defaultProxy> element
ms.assetid: 9d663c4b-07b4-4f6f-9b12-efbd3630354f
ms.openlocfilehash: 806a30a52219ef9185f84a650d6a8eef8fb0dc8c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91190301"
---
# <a name="defaultproxy-element-network-settings"></a>\<defaultProxy> 元素（网络设置）

配置超文本传输协议 (HTTP) 代理服务器。  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<defaultProxy>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<defaultProxy  
  enabled="True|False"  
  useDefaultCredentials="True|False">  
    <bypasslist>...</bypasslist>  
    <proxy>...</proxy>  
    <module>...</module>  
</defaultProxy>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|`enabled`|指定是否使用 Web 代理。 默认值是 `True`。|  
|`useDefaultCredentials`|指定是否使用此主机的默认凭据来访问 Web 代理。 默认值是 `False`。|  
  
### <a name="child-elements"></a>子元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[bypasslist](bypasslist-element-network-settings.md)|提供一组描述不使用代理的地址的正则表达式。|  
|[模块](module-element-network-settings.md)|向应用程序添加新的代理模块。|  
|[代理](proxy-element-network-settings.md)|定义代理服务器。|  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[system.net](system-net-element-network-settings.md)|包含指定 .NET Framework 如何连接到网络的设置。|  
  
## <a name="remarks"></a>备注  

 如果 defaultProxy 元素为空，则将沿用 Internet Explorer 中的代理设置。 这种行为在 .NET Framework 1.1 版中有所不同。  
  
 如果 [module](module-element-network-settings.md) 元素指定非公共类型，该类型不是从 <xref:System.Net.IWebProxy> 类派生，则此对象的无参数构造函数中出现异常，或者在检索系统指定的默认代理时出现异常，则会引发异常。 异常的 <xref:System.Exception.InnerException%2A> 属性应具有错误根本原因的详细信息。  
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="example"></a>示例  

 以下示例使用 Internet Explorer 代理中的默认值，指定代理地址，并绕过代理进行本地访问和 contoso.com。  
  
```xml  
<configuration>  
  <system.net>  
    <defaultProxy>  
      <proxy  
        usesystemdefault="True"  
        proxyaddress="http://192.168.1.10:3128"  
        bypassonlocal="True"  
      />  
      <bypasslist>  
        <add address="[a-z]+\.contoso\.com$" />  
      </bypasslist>  
    </defaultProxy>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [网络设置架构](index.md)
