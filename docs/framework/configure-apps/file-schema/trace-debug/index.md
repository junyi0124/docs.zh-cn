---
title: 跟踪和调试设置架构
ms.date: 03/30/2017
helpviewer_keywords:
- tracing [.NET Framework], trace and debug settings schema
- configuration schema [.NET Framework], trace and debug settings
- configuration settings [.NET Framework], tracing
- schema configuration settings
- configuration settings [.NET Framework], debugging
- trace listeners, trace and debug settings schema
- configuration sections [.NET Framework]
- elements [.NET Framework], trace and debug settings
ms.assetid: 277ca5f6-e1c4-41b6-a47f-3a67ce5b94ac
ms.openlocfilehash: ae089e941d75df7ba1cd87b5b92a514a33bfbf85
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91195189"
---
# <a name="trace-and-debug-settings-schema"></a>跟踪和调试设置架构

跟踪和调试设置指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。  
  
 下表介绍每个跟踪和调试设置元素的功能。  
  
|元素|描述|  
|-------------|-----------------|  
|[\<add>](add-element-for-listeners-for-source.md)|将侦听器添加到跟踪源的 `Listeners` 集合中。|  
|[\<add>](add-element-for-listeners-for-trace.md)|将侦听器添加到 `Listeners` 集合中。|  
|[\<add>](add-element-for-sharedlisteners.md)|将侦听器添加到 `sharedListeners` 集合中。|  
|[\<add>](add-element-for-switches.md)|指定对跟踪开关设置的级别。|  
|[\<assert>](assert-element.md)|指定调用 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> 方法时是否显示消息框；另外指定要写入消息的文件的名称。|  
|[\<clear>](clear-element-for-listeners-for-source.md)|清除跟踪源的 `Listeners` 集合。|  
|[\<clear>](clear-element-for-listeners-for-trace.md)|清除跟踪的 `Listeners` 集合。|  
|[\<filter>](filter-element-for-add-for-listeners-for-source.md)|将筛选器添加到跟踪源的 `Listeners` 集合中的侦听器。|  
|[\<filter>](filter-element-for-add-for-listeners-for-trace.md)|将筛选器添加到跟踪的 `Listeners` 集合中的侦听器。|  
|[\<filter>](filter-element-for-add-for-sharedlisteners.md)|将筛选器添加到 `sharedListeners` 集合中的侦听器。|  
|[\<listeners>](listeners-element-for-source.md)|指定跟踪源的 `Listeners` 集合中的侦听器。|  
|[\<listeners>](listeners-element-for-trace.md)|指定跟踪的 `Listeners` 集合中的侦听器。|  
|[\<performanceCounters>](performancecounters-element.md)|指定由性能计数器共享的全局内存的大小。|  
|[\<remove>](remove-element-for-listeners-for-trace.md)|从跟踪的 `Listeners` 集合中删除侦听器。|  
|[\<remove>](remove-element-for-listeners-for-source.md)|从跟踪源的 `Listeners` 集合中删除侦听器。|  
|[\<sharedListeners>](sharedlisteners-element.md)|包含任何源或跟踪元素可以引用的侦听器。|  
|[\<sources>](sources-element.md)|包含用于启动跟踪消息的跟踪源。|  
|[\<source>](source-element.md)|指定用于启动跟踪消息的跟踪源。|  
|[\<switches>](switches-element.md)|包含跟踪开关和对该跟踪开关设置的级别。|  
|[\<system.diagnostics>](system-diagnostics-element.md)|指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。|  
|[\<trace>](trace-element.md)|包含用于收集、存储和路由跟踪消息的侦听器。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.Debug>
- [配置文件架构](../index.md)
