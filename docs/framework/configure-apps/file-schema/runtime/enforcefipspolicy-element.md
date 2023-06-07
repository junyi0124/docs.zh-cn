---
title: <enforceFIPSPolicy> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- enforceFIPSPolicy element
- FIPS
- <enforceFIPSPolicy> element
- Federal Information Processing Standards (FIPS)
ms.assetid: c35509c4-35cf-43c0-bb47-75e4208aa24e
ms.openlocfilehash: 864a371d4ad10585e672452ad85cc09d4b684068
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158833"
---
# <a name="enforcefipspolicy-element"></a>\<enforceFIPSPolicy> 元素

指定是否强制执行以下计算机配置要求：加密算法必须符合美国联邦信息处理标准 (FIPS)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<enforceFIPSPolicy>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<enforceFIPSPolicy enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|enabled|必需的特性。<br /><br /> 指定是否允许强制执行计算机配置要求，要求加密算法必须符合 FIPS。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|`true`|如果你的计算机配置为要求加密算法符合 FIPS 标准，则强制实施此要求。 如果某个类实现了不符合 FIPS 的算法，则该类的构造函数或方法将在 `Create` 该计算机上运行时引发异常。 这是默认设置。|  
|`false`|无论计算机配置如何，应用程序使用的加密算法都无需符合 FIPS。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 从 .NET Framework 2.0 开始，实现加密算法的类的创建由计算机的配置控制。 如果计算机配置为要求算法符合 FIPS，并且类实现了不符合 FIPS 的算法，则创建该类的实例的任何尝试都将引发异常。 构造函数引发 <xref:System.InvalidOperationException> 异常，并且 `Create` 方法引发异常，并出现 <xref:System.Reflection.TargetInvocationException> 内部 <xref:System.InvalidOperationException> 异常。  
  
 如果你的应用程序运行在其配置要求符合 FIPS 的计算机上，并且你的应用程序使用不符合 FIPS 的算法，则可以使用配置文件中的此元素来阻止公共语言运行时 (CLR) 强制执行 FIPS 相容性。 此元素是在 .NET Framework 2.0 Service Pack 1 中引入的。  
  
## <a name="example"></a>示例  

 下面的示例演示如何阻止 CLR 强制实施 FIPS 相容性。  
  
```xml  
<configuration>  
    <runtime>  
        <enforceFIPSPolicy enabled="false"/>  
    </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [加密模型](../../../../standard/security/cryptography-model.md)
