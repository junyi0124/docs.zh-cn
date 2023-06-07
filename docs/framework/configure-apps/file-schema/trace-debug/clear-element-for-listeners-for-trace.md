---
title: <clear>的元素 <listeners><trace>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners/clear
helpviewer_keywords:
- clear element for <listeners> for <trace>
- <clear> element for <listeners> for <trace>
ms.assetid: b44732a8-271f-4a06-ba9e-fe3298d6f192
ms.openlocfilehash: 2d301d588e33b0eea4164a6bf62dedd7b0c450ec
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91149265"
---
# <a name="clear-element-for-listeners-for-trace"></a>\<clear>的元素 \<listeners>\<trace>

清除跟踪的 `Listeners` 集合。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<trace>**](trace-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<listeners>**](listeners-element-for-trace.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**

## <a name="syntax"></a>语法  
  
```xml  
<clear/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.diagnostics`|指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。|  
|`trace`|包含用于收集、存储和路由跟踪消息的侦听器。|  
|`listeners`|包含收集、存储和路由消息的侦听器。 侦听器将跟踪输出定向到适当的目标。|  
  
## <a name="remarks"></a>备注  

 `<clear>`元素从跟踪的集合中移除所有侦听器 `Listeners` 。 可以在使用元素 `<clear>` 之前使用元素 `<add>` ，以确定集合中没有其他活动的侦听器。  
  
 您可以 `Listeners` 通过 <xref:System.Diagnostics.TraceListenerCollection.Clear%2A> 对 <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> 属性 () 调用方法，以编程方式清除该集合 `System.Diagnostics.Trace.Listeners.Clear()` 。  
  
 此元素可在计算机配置文件中使用 ( # A0) 和应用程序配置文件。  
  
> [!NOTE]
> `<clear>`元素 <xref:System.Diagnostics.DefaultTraceListener> 从集合中移除， `Listeners` 并更改 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> 、 <xref:System.Diagnostics.Trace.Assert%2A?displayProperty=nameWithType> 、 <xref:System.Diagnostics.Debug.Fail%2A?displayProperty=nameWithType> 和 <xref:System.Diagnostics.Trace.Fail%2A?displayProperty=nameWithType> 方法的行为。 通常调用 `Assert` 或 `Fail` 方法会导致显示消息框。 但是，如果不在集合中，则不会显示消息框 <xref:System.Diagnostics.DefaultTraceListener> `Listeners` 。  
  
## <a name="example"></a>示例  

 下面的示例演示如何 `<clear>` 在使用 `<add>` 元素将侦听器添加 `console` 到用于 trace 的集合之前使用元素 `Listeners` 。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <trace autoflush="false" indentsize="4">  
      <listeners>  
        <clear/>  
        <add name="console"
          type="System.Diagnostics.ConsoleTraceListener" >  
          <filter type="System.Diagnostics.EventTypeFilter"
            initializeData="Error" />  
        </add>  
      </listeners>  
    </trace>  
  </system.diagnostics>  
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.Trace.Listeners%2A>
- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.Debug>
- <xref:System.Diagnostics.TraceSource>
- [跟踪和调试设置架构](index.md)
- [\<remove>](remove-element-for-listeners-for-trace.md)
- [跟踪侦听器](../../../debug-trace-profile/trace-listeners.md)
