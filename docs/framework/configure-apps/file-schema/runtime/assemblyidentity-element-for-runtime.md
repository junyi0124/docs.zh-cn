---
title: <runtime> 的 <assemblyIdentity> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/dependentAssembly/assemblyIdentity
- http://schemas.microsoft.com/.NetConfiguration/v2.0#assemblyIdentity
helpviewer_keywords:
- <assemblyIdentity> element
- container tags, <assemblyIdentity> element
- assemblyIdentity element
ms.assetid: cea4d187-6398-4da4-af09-c1abc6a349c1
ms.openlocfilehash: f3e74b05ac0fd7c57963f2aad047ba3f2d63a10a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170176"
---
# <a name="assemblyidentity-element-for-runtime"></a>\<runtime> 的 \<assemblyIdentity> 元素

包含有关程序集的标识信息。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<assemblyBinding>**](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dependentAssembly>**](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<assemblyIdentity>**  
  
## <a name="syntax"></a>语法  
  
```xml  
   <assemblyIdentity
name="assembly name"  
publicKeyToken="public key token"  
culture="assembly culture"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`name`|必需的特性。<br /><br /> 程序集的名称|  
|`culture`|可选特性。<br /><br /> 一个字符串，指定程序集的语言和国家/地区。|  
|`publicKeyToken`|可选特性。<br /><br /> 一个十六进制值，该值指定程序集的强名称。|  
|`processorArchitecture`|可选特性。<br /><br /> 值 "x86"、"amd64"、"msil" 或 "ia64" 之一，为包含特定于处理器的代码的程序集指定处理器体系结构。 这些值不区分大小写。 如果为该属性分配了其他任何值，则将 `<assemblyIdentity>` 忽略整个元素。 请参阅 <xref:System.Reflection.ProcessorArchitecture>。|  
  
## <a name="processorarchitecture-attribute"></a>processorArchitecture 特性  
  
|值|描述|  
|-----------|-----------------|  
|`amd64`|仅适用于 AMD x86-64 体系结构。|  
|`ia64`|仅限 Intel Itanium 体系结构。|  
|`msil`|不特定于处理器和每字位数。|  
|`x86`|32位 x86 处理器，在 Windows on Windows (WOW) 环境的64位平台上。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`assemblyBinding`|包含有关程序集版本重定向和程序集位置的信息。|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`dependentAssembly`|封装每个程序集的绑定策略和程序集位置。 `<dependentAssembly>`为每个程序集使用一个元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 每个 **\<dependentAssembly>** 元素必须有一个 **\<assemblyIdentity>** 子元素。  
  
 如果该 `processorArchitecture` 属性存在，则 `<assemblyIdentity>` 元素仅适用于具有相应处理器体系结构的程序集。 如果该 `processorArchitecture` 属性不存在，则该 `<assemblyIdentity>` 元素可应用于具有任何处理器体系结构的程序集。  
  
 下面的示例显示了两个程序集的配置文件，这些程序集具有两个不同的两个处理器体系结构，并且其版本尚未保持同步。当应用程序在 x86 平台上执行时，第一个 `<assemblyIdentity>` 元素应用，另一个元素将被忽略。 如果应用程序在 x86 或 ia64 以外的平台上执行，则会忽略这两者。  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="MyAssembly"  
                  publicKeyToken="14a739be0244c389"  
                  culture="neutral"  
                  processorArchitecture="x86" />  
            <bindingRedirect oldVersion= "1.0.0.0"
                  newVersion="1.1.0.0" />  
         </dependentAssembly>  
         <dependentAssembly>  
            <assemblyIdentity name="MyAssembly"  
                  publicKeyToken="14a739be0244c389"  
                  culture="neutral"
                  processorArchitecture="ia64" />  
            <bindingRedirect oldVersion="1.0.0.0"
                  newVersion="2.0.0.0" />  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
 如果配置文件包含 `<assemblyIdentity>` 没有属性的元素 `processorArchitecture` ，并且不包含与平台相匹配的元素，则使用没有属性的元素 `processorArchitecture` 。  
  
## <a name="example"></a>示例  

 下面的示例演示如何提供有关程序集的信息。  
  
```xml  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <!--Redirection and codeBase policy for myAssembly.-->  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [重定向程序集版本](../../redirect-assembly-versions.md)
