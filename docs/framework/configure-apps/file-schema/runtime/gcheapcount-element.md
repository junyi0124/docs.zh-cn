---
title: GCHeapCount 元素
ms.date: 11/08/2019
helpviewer_keywords:
- gcHeapCount element
- <gcHeapCount> element
ms.openlocfilehash: 3d6cac4185af182758cb82e6bfd9d96ed24869b4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "74283074"
---
# <a name="gcheapcount-element"></a>\<GCHeapCount> 元素

指定用于服务器垃圾回收的堆/线程数。

\<configuration>\
&nbsp;&nbsp;\<runtime>\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCHeapCount>

## <a name="syntax"></a>语法

```xml
<GCHeapCount
   enabled="nn"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br />指定用于服务器垃圾回收的堆数。 堆的实际数量是指定的堆数的最小值以及允许进程使用的处理器数。 |

#### <a name="enabled-attribute"></a>enabled 属性

|值|说明|
|-----------|-----------------|
|`nn`|要用于服务器 GC 的堆数。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|

## <a name="remarks"></a>注解

默认情况下，服务器 GC 线程使用各自的 CPU 进行关联，以便为每个处理器提供一个 GC 堆、一个服务器 GC 线程，以及一个后台服务器垃圾回收线程。 从 .NET Framework 4.6.2 开始，可以使用**GCHeapCount**元素来限制应用程序用于服务器 GC 的堆数。 如果运行多个服务器应用程序实例的系统，限制用于服务器 GC 的堆数将特别有用。

**GCHeapCount**通常与其他两个标志一起使用：

- [GCNoAffinitize](gcnoaffinitize-element.md)，控制服务器 GC 线程/堆是否与 cpu 关联。

- [GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)，它控制垃圾回收器与 cpu 之间的关联。

如果设置了**GCHeapCount**并禁用了**GCNoAffinitize** （其默认设置），则*nn* GC threads/堆和第一个*nn*处理器之间存在关联。 您可以使用**GCHeapAffinitizeMask**元素指定进程的服务器 GC 堆使用的处理器。 否则，如果在一个系统上运行多个服务器进程，其处理器使用率将会重叠。

如果设置了**GCHeapCount**并启用了**GCNoAffinitize** ，则垃圾回收器会限制服务器 gc 使用的处理器数量，但不会关联 GC 堆和处理器。

## <a name="example"></a>示例

下面的示例指示应用程序使用具有10个堆/线程的服务器 GC。 由于你不希望这些堆与系统上运行的其他应用程序中的堆重叠，因此你可以使用**GCHeapAffinitizeMask**来指定该进程应使用 cpu 0 到9。

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCHeapAffinitizeMask enabled="1023"/>
   </runtime>
</configuration>
```

以下示例不关联服务器 GC 线程，并将 GC 堆/线程的数量限制为10个。

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [GCNoAffinitize 元素](gcnoaffinitize-element.md)
- [GCHeapAffinitizeMask 元素](gcheapaffinitizemask-element.md)
- [垃圾回收的基本知识](../../../../standard/garbage-collection/fundamentals.md)
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
