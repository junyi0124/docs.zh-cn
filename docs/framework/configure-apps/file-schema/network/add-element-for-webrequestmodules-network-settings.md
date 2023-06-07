---
title: webRequestModules 的 <add> 元素（网络设置）
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/webRequestModules/add
- http://schemas.microsoft.com/.NetConfiguration/v2.0#add
helpviewer_keywords:
- <webRequestModules>, add element
- webRequestModules, add element
- add element, webRequestModules
- <add> element, webRequestModules
ms.assetid: 47ec4adc-f39f-4bcd-8680-1ec21fd26890
ms.openlocfilehash: 8d792b967d967540469dca7c090e0f905ecb2e6b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201754"
---
# <a name="add-element-for-webrequestmodules-network-settings"></a>webRequestModules 的 \<add> 元素（网络设置）

向应用程序添加自定义 Web 请求模块。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<webRequestModules>**](webrequestmodules-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**

## <a name="syntax"></a>语法  
  
```xml  
<add
  prefix="URI prefix"
  type="type_fullname, assembly_fullname"
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|**Attribute**|**说明**|  
|-------------------|---------------------|  
|`prefix`|此 Web 请求模块处理的请求的 URI 前缀。|  
|`type`|完全限定的类型名称 (由) 的 <xref:System.Type.FullName%2A> 属性和程序集名称指示 (由 <xref:System.Reflection.Assembly.FullName%2A> 实现此 Web 请求模块的由逗号分隔的) 属性指示。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|**元素**|**说明**|  
|-----------------|---------------------|  
|[webRequestModules](webrequestmodules-element-network-settings.md)|指定用于从网络主机请求信息的模块。|  
  
## <a name="remarks"></a>备注  

 `prefix`特性定义使用指定 Web 请求模块的 URI 前缀。 通常会将 Web 请求模块注册为处理特定协议（如 HTTP 或 FTP），但可以注册它来处理对服务器上特定服务器或路径的请求。  
  
 当 URI 匹配前缀传递到方法时，将创建 Web 请求模块 <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> 。  
  
 特性的值 `prefix` 应为有效 URI 的前导字符。 例如，`http` 或 `http://www.contoso.com`。
  
 特性的值 `type` 应为有效的类型名称和相应的程序集名称，用逗号分隔。
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="example"></a>示例  

 下面的示例为 HTTP 注册自定义 Web 请求模块。 应将版本和 PublicKeyToken 的值替换为指定模块的正确值。  
  
```xml  
<configuration>  
  <system.net>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator, System, Version=2.0.3600.0,  
           Culture=neutral, PublicKeyToken=b77a5c561934e089"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.WebRequest>
- [网络设置架构](index.md)
