---
title: <remove>的元素 <listeners><source>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sources/source/listeners/remove
helpviewer_keywords:
- remove element for <listeners> for <source>
- <remove> element for <listeners> for <source>
ms.assetid: 3ff6b578-273d-407f-b07f-8251f1f9f5d0
ms.openlocfilehash: 53ba773ea1cb31955e59c1f57e1c0cc807227402
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173868"
---
# <a name="remove-element-for-listeners-for-source"></a>\<remove>的元素 \<listeners>\<source>

从跟踪源的 `Listeners` 集合中删除侦听器。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<sources>**](sources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<source>**](source-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<listeners>**](listeners-element-for-source.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<remove>**

## <a name="syntax"></a>语法  
  
```xml  
<remove name="listenerName" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`name`|必需的特性。<br /><br /> 要从集合中删除的侦听器的名称 `Listeners` 。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.diagnostics`|指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。|  
|`sources`|包含用于启动跟踪消息的跟踪源。|  
|`source`|指定用于启动跟踪消息的跟踪源。|  
|`listeners`|指定用于收集、存储和路由消息的侦听器。|  
  
## <a name="remarks"></a>备注  

 `<remove>`元素从跟踪源的集合中删除指定的侦听器 `Listeners` 。  
  
 您可以 `Listeners` 通过 <xref:System.Diagnostics.TraceListenerCollection.Remove%2A> 对实例的属性调用方法，以编程方式从跟踪源的集合中移除一个元素 <xref:System.Diagnostics.TraceSource.Listeners%2A> <xref:System.Diagnostics.TraceSource> 。  
  
 此元素可在计算机配置文件中使用 ( # A0) 和应用程序配置文件。  
  
## <a name="example"></a>示例  

 下面的示例演示如何 `<remove>` 在使用 `<add>` 元素将侦听器添加 `console` 到 `Listeners` 跟踪源的集合之前使用元素 `TraceSourceApp` 。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <source name="TraceSourceApp" switchName="sourceSwitch"
         switchType="System.Diagnostics.SourceSwitch" >  
         <listeners>  
           <remove name="Default"/>  
           <add name="console"
             type="System.Diagnostics.ConsoleTraceListener" />  
         </listeners>  
      </source>  
    </sources>  
  </system.diagnostics>  
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.TraceSource.Listeners%2A>
- <xref:System.Diagnostics.TraceSource>
- [跟踪和调试设置架构](index.md)
- [\<clear>](clear-element-for-listeners-for-source.md)
- [跟踪侦听器](../../../debug-trace-profile/trace-listeners.md)
