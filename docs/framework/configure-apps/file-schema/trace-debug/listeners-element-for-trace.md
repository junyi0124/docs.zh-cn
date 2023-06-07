---
title: <trace> 的 <listeners> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners
helpviewer_keywords:
- <listeners> element
- listeners element
ms.assetid: 1394c2c3-6304-46db-87c1-8e8b16f5ad5b
ms.openlocfilehash: 59d078f8dc573a1ce949d225f497dd4500fe808f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173856"
---
# <a name="listeners-element-for-trace"></a>\<trace> 的 \<listeners> 元素

指定用于收集、存储和路由消息的侦听器。 侦听器将跟踪输出定向到适当的目标。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<trace>**](trace-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<listeners>**

## <a name="syntax"></a>语法  
  
```xml  
<listeners>
  <add>...</add>  
  <clear/>  
  <remove ... />  
</listeners>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<add>](add-element-for-listeners-for-trace.md)|将侦听器添加到 `Listeners` 集合中。|  
|[\<clear>](clear-element-for-listeners-for-trace.md)|清除跟踪的 `Listeners` 集合。|  
|[\<remove>](remove-element-for-listeners-for-trace.md)|从集合中移除侦听器 `Listeners` 。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.diagnostics`|为 ASP.NET 配置节指定根元素。|  
|`trace`|包含用于收集、存储和路由跟踪消息的侦听器。|  
  
## <a name="remarks"></a>备注  

 <xref:System.Diagnostics.Debug>和 <xref:System.Diagnostics.Trace> 类共享相同的**侦听器**集合。 如果将侦听器对象添加到其中一个类的集合中，则其他类将使用同一侦听器。 随 .NET Framework 附带的侦听器类派生自 <xref:System.Diagnostics.TraceListener> 类。  
  
## <a name="configuration-file"></a>配置文件  

 此元素可在计算机配置文件中使用 ( # A0) 和应用程序配置文件。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 **\<listeners>** 元素将侦听器 `MyListener` 和添加 `MyEventListener` 到 **侦听器** 集合。 `MyListener` 创建一个名为的文件 `MyListener.log` ，并将输出写入文件。 `MyEventListener` 在事件日志中创建一个条目。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <trace autoflush="true" indentsize="0">  
      <listeners>  
        <add name="myListener"
          type="System.Diagnostics.TextWriterTraceListener,
            system, version=1.0.3300.0, Culture=neutral,
            PublicKeyToken=b77a5c561934e089"
          initializeData="c:\myListener.log" />  
        <add name="MyEventListener"  
          type="System.Diagnostics.EventLogTraceListener,
            system, version=1.0.3300.0, Culture=neutral,
            PublicKeyToken=b77a5c561934e089"  
          initializeData="MyConfigEventLog"/>  
      </listeners>  
    </trace>  
  </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.TraceListener>
- [跟踪和调试设置架构](index.md)
