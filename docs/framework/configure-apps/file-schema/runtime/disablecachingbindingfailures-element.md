---
title: <disableCachingBindingFailures> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#disableCachingBindingFailures
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/disableCachingBindingFailures
helpviewer_keywords:
- assemblies [.NET Framework],caching binding failures
- caching assembly binding failures
- <disableCachingBindingFailures> element
- disableCachingBindingFailures element
ms.assetid: bf598873-83b7-48de-8955-00b0504fbad0
ms.openlocfilehash: c9e608bfd54b641564a9095076455e10dd8653fb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176118"
---
# <a name="disablecachingbindingfailures-element"></a>\<disableCachingBindingFailures> 元素

指定是否禁止缓存发生的绑定故障，因为探查找不到该程序集。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<disableCachingBindingFailures>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<disableCachingBindingFailures enabled="0|1"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|enabled|必需的特性。<br /><br /> 指定是否禁止缓存发生的绑定故障，因为探查找不到该程序集。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|0|请勿禁止缓存发生的绑定故障，因为探查找不到该程序集。 这是默认绑定行为，从 .NET Framework 版本2.0 开始。|  
|1|禁止缓存发生的绑定故障，因为探查找不到该程序集。 此设置将恢复为 .NET Framework 版本1.1 的绑定行为。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 从 .NET Framework 版本2.0 开始，加载程序集的默认行为是缓存所有绑定和加载失败。 也就是说，如果尝试加载程序集失败，则加载同一程序集的后续请求将立即失败，而不会尝试查找程序集。 此元素将禁用发生的绑定故障的默认行为，因为在探测路径中找不到该程序集。 这些失败会引发 <xref:System.IO.FileNotFoundException> 。  
  
 某些绑定和加载失败不受此元素影响，并且始终会进行缓存。 出现这些失败是因为程序集已找到，但无法加载。 它们引发 <xref:System.BadImageFormatException> 或 <xref:System.IO.FileLoadException> 。 以下列表包括此类故障的一些示例。  
  
- 如果尝试加载文件不是有效的程序集，则即使将错误文件替换为正确的程序集，以后尝试加载程序集也会失败。  
  
- 如果尝试加载文件系统锁定的程序集，则即使在文件系统释放程序集之后，加载程序集的后续尝试也将失败。  
  
- 如果尝试加载的程序集的一个或多个版本位于探测路径中，但你请求的特定版本不在其中，则即使将正确的版本移入探测路径，后续尝试加载该版本也会失败。  
  
## <a name="example"></a>示例  

 下面的示例演示如何禁止缓存发生的程序集绑定故障，因为探查找不到该程序集。  
  
```xml  
<configuration>  
   <runtime>  
      <disableCachingBindingFailures enabled="1" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [运行时如何定位程序集](../../../deployment/how-the-runtime-locates-assemblies.md)
