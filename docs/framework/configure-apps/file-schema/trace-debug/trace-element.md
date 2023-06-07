---
title: <trace> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace
- http://schemas.microsoft.com/.NetConfiguration/v2.0#trace
helpviewer_keywords:
- <trace> element
- listeners
- trace element
- trace listener, <trace> element
ms.assetid: 7931c942-63c1-47c3-a045-9d9de3cacdbf
ms.openlocfilehash: 617b42a0be2be272a78b33be997cce632d1c6dcb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198920"
---
# <a name="trace-element"></a>\<trace> 元素

包含用于收集、存储和路由跟踪消息的侦听器。  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<trace>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<trace autoflush="true|false"
       indentsize="indent value"  
       useGlobalLock="true| false"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`autoflush`|可选特性。<br /><br /> 指定跟踪侦听器是否在每次写入操作后自动刷新输出缓冲区。|  
|`indentsize`|可选特性。<br /><br /> 指定缩进的空格数。|  
|`useGlobalLock`|可选特性。<br /><br /> 指示是否应使用全局锁。|  
  
## <a name="autoflush-attribute"></a>autoflush 特性  
  
|值|描述|  
|-----------|-----------------|  
|`false`|不会自动刷新输出缓冲区。 这是默认设置。|  
|`true`|自动刷新输出缓冲区。|  
  
## <a name="usegloballock-attribute"></a>useGlobalLock 特性  
  
|值|描述|  
|-----------|-----------------|  
|`false`|如果侦听器是线程安全的，则不使用全局锁定;否则，将使用全局锁。|  
|`true`|无论侦听器是否是线程安全的，都使用全局锁。 这是默认设置。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<listeners>](listeners-element-for-trace.md)|指定用于收集、存储和路由消息的侦听器。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.diagnostics`|指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。|  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 `<trace>` 元素将侦听器添加 `MyListener` 到 `Listeners` 集合中。 `MyListener` 创建一个名为的文件 `MyListener.log` ，并将输出写入文件。 `useGlobalLock` `false` 如果跟踪侦听器是线程安全的，则属性设置为，这将导致不会使用全局锁。 `autoflush`特性设置为 `true` ，这将导致跟踪侦听器写入文件，而不管是否 <xref:System.Diagnostics.Trace.Flush%2A?displayProperty=nameWithType> 调用了方法。 `indentsize`特性设置为 0 (零) ，这将导致侦听器在调用方法时缩进零个空格 <xref:System.Diagnostics.Trace.Indent%2A?displayProperty=nameWithType> 。  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <trace useGlobalLock="false" autoflush="true" indentsize="0">  
         <listeners>  
            <add name="myListener" type="System.Diagnostics.TextWriterTraceListener, system version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />  
         </listeners>  
      </trace>  
   </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.TraceListener>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- [跟踪和调试设置架构](index.md)
