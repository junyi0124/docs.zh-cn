---
title: <system.Net> 元素（网络设置）
description: <system.Net> 网络设置元素包含用于指定 .NET Framework 如何连接到 .NET Framework 中的网络选项的设置。
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#system.Net
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.Net
helpviewer_keywords:
- system.Net element
- <system.Net> element
ms.assetid: 52de4d6c-b24d-44aa-ba7d-6b5061f1357e
ms.openlocfilehash: 80d54df40c6798e146013b4f2d867386ae35169c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201715"
---
# <a name="systemnet-element-network-settings"></a>\<system.Net> 元素（网络设置）

包含指定 .NET Framework 如何连接到网络的设置。  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<system.net>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.net>
</system.net>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[authenticationModules](authenticationmodules-element-network-settings.md)|指定用于对 Internet 请求进行身份验证的模块。|  
|[connectionManagement](connectionmanagement-element-network-settings.md)|指定与 Internet 主机的最大连接数。|  
|[defaultProxy](defaultproxy-element-network-settings.md)|配置超文本传输协议 (HTTP) 代理服务器。|  
|[mailSettings](mailsettings-element-network-settings.md)| (SMTP) 邮件发送选项配置简单邮件传输协议。|  
|[requestCaching](requestcaching-element-network-settings.md)|控制网络请求的缓存机制。|  
|[设置](settings-element-network-settings.md)|为 <xref:System.Net> 和相关子命名空间中的类配置基本网络选项。|  
|[webRequestModules](webrequestmodules-element-network-settings.md)|指定用于从 Internet 主机请求信息的模块。|  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[configuration](../configuration-element.md)|包含所有命名空间的设置。|  
  
## <a name="remarks"></a>备注  

 [\<system.net>](system-net-element-network-settings.md)元素包含 <xref:System.Net> 和相关子命名空间中的类的设置。 设置配置身份验证模块、连接管理、邮件设置、代理服务器和 Internet 请求模块，用于接收来自 Internet 主机的信息。  
  
## <a name="example"></a>示例  

 下面的示例演示了类使用的典型配置 <xref:System.Net> 。  
  
```xml  
<configuration>  
  <system.net>  
    <authenticationModules>  
      <add type="System.Net.DigestClient" />  
      <add type="System.Net.NegotiateClient" />  
      <add type="System.Net.KerberosClient" />  
      <add type="System.Net.NtlmClient" />  
      <add type="System.Net.BasicClient" />  
    </authenticationModules>  
    <connectionManagement>  
      <add address="*" maxconnection="2" />  
    </connectionManagement>  
    <defaultProxy>  
      <proxy  
        usesystemdefault="true"  
        bypassonlocal="true"  
      />  
    </defaultProxy>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator"  
      />  
      <add prefix="https"  
           type="System.Net.HttpRequestCreator"  
      />  
      <add prefix="file"  
           type="System.Net.FileWebRequestCreator"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [网络设置架构](index.md)
