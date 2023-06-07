---
title: <customCookieHandler>
ms.date: 03/30/2017
ms.assetid: a03b153d-5ec6-4915-9031-6f0c3fd348be
author: BrucePerlerMS
ms.openlocfilehash: a8ee23ac6facaea18cd7f1820616cb9b16afa336
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189846"
---
# \<customCookieHandler>

设置自定义 cookie 处理程序类型。 仅当 `mode` 元素的属性 `<cookieHandler>` 为 "Custom" 时，此元素才能存在。 自定义类型必须派生自 <xref:System.IdentityModel.Services.CookieHandler> 类。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<federationConfiguration>**](federationconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cookieHandler>**](cookiehandler.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<customCookieHandler>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration>  
    <cookieHandler mode="Custom">  
      <customCookieHandler type="MyNamespace.MyCustomCookieHandler, MyAssembly" >  
      </customCookieHandler>  
    </cookieHandler>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|type|指定从类派生的自定义类型 <xref:System.IdentityModel.Services.CookieHandler> 。 有关如何指定属性的详细信息 `type` ，请参阅 [自定义类型引用](../windows-workflow-foundation/index.md)。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<cookieHandler>](cookiehandler.md)|配置用于 <xref:System.IdentityModel.Services.CookieHandler> <xref:System.IdentityModel.Services.SessionAuthenticationModule> 读取和写入 cookie 的。|  
  
## <a name="remarks"></a>备注  

 如果通过将元素的属性设置为 "Custom" 来指定自定义 cookie 处理程序 `mode` `<cookieHandler>` ，则必须通过包含 `<customCookieHandler>` 引用 cookie 处理程序类型的子元素来指定自定义 cookie 处理程序的类型。 当 `mode` 特性设置为 "分块" 或 "Default" 时，不能指定此元素。 自定义 cookie 处理程序必须派生自 <xref:System.IdentityModel.Services.CookieHandler> 类。  
  
 `<customCookieHandler>`元素由 <xref:System.IdentityModel.Configuration.CustomTypeElement> 类表示。  
  
## <a name="example"></a>示例  

 下面的示例将 SAM 配置为使用类型的自定义 cookie 处理程序 `MyNamespace.MyCustomCookieHandler` 。  
  
```xml  
<cookieHandler mode="Custom">  
    <customCookieHandler type="MyNamespace.MyCustomCookieHandler, MyAssembly" />  
</cookieHandler>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Services.CookieHandler>
