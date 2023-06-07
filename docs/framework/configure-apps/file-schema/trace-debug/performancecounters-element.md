---
title: <performanceCounters> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/performanceCounters
- http://schemas.microsoft.com/.NetConfiguration/v2.0#performanceCounters
helpviewer_keywords:
- performanceCounters element
- <performanceCounters> element
ms.assetid: a71f605b-c7d9-4501-a5c3-abcbb964a43f
ms.openlocfilehash: f52fdb2d5b0b7911de63f96663e70735d2f2496c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "71697156"
---
# <a name="performancecounters-element"></a>\<performanceCounters> 元素

指定由性能计数器共享的全局内存的大小。

[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<performanceCounters>**  

## <a name="syntax"></a>语法

```xml
<performanceCounters filemappingsize="524288" />
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|filemappingsize|必需的特性。<br /><br /> 指定由性能计数器共享的全局内存的大小（以字节为单位）。 默认值为 524288。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`Configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`system.diagnostics`|为 ASP.NET 配置节指定根元素。|

## <a name="remarks"></a>注解

性能计数器使用内存映射文件或共享内存来发布性能数据。  共享内存的大小决定了可同时使用的实例数。  共有两种类型的共享内存：全局共享内存和单独的共享内存。  与 .NET Framework 版本1.0 或1.1 一起安装的所有性能计数器类别均使用全局共享内存。  随 .NET Framework 版本2.0 一起安装的性能计数器类别使用单独的共享内存，其中每个性能计数器类别都有自己的内存。

全局共享内存的大小只能与配置文件一起设置。  默认大小为 524288 byes，最大大小为33554432个字节，最小大小为32768个字节。  由于全局共享内存由所有进程和类别共享，因此第一个创建者指定大小。  如果在应用程序配置文件中定义大小，则仅当应用程序是导致性能计数器执行的第一个应用程序时，才使用该大小。  因此，指定值的正确位置 `filemappingsize` 是 machine.config 文件。  全局共享内存中的内存不能由单独的性能计数器释放，因此，如果创建了大量具有不同名称的性能计数器实例，则最终的全局共享内存会耗尽。

对于单独的共享内存的大小，先引用注册表项 HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services \Performance 中的 DWORD FileMappingSize 值 \\ *\<category name>* ，然后引用在配置文件中为全局共享内存指定的值。 如果 FileMappingSize 值不存在，则会在配置文件中将单独的共享内存大小设置为第四（1/4）个全局设置。

## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.PerformanceCounter>
- <xref:System.Diagnostics.PerformanceCounterCategory>
- <xref:System.Diagnostics.PerformanceCounter.InstanceLifetime%2A>
- <xref:System.Diagnostics.PerformanceCounterInstanceLifetime>
