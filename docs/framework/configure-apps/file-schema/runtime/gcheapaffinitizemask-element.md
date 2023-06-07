---
title: GCHeapAffinitizeMask 元素
ms.date: 11/08/2019
helpviewer_keywords:
- gcHeapCount element
- <gcHeapCount> element
ms.openlocfilehash: 09d6523fb10692dd3617a3827d5bccf112bc632b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73978419"
---
# <a name="gcheapaffinitizemask-element"></a>\<GCHeapAffinitizeMask> 元素

定义 GC 堆和单个处理器之间的关联。

\<configuration>\
&nbsp;&nbsp;\<runtime>\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCHeapAffinitizeMask>

## <a name="syntax"></a>语法

```xml
<GCHeapAffinitizeMask
   enabled="nnnn"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br />指定 GC 堆和单个处理器之间的相关性。 |

#### <a name="enabled-attribute"></a>enabled 属性

|值|说明|
|-----------|-----------------|
|`nnnn`|一个十进制值，它构成一个位掩码，用于定义服务器 GC 堆和单个处理器之间的相关性。 |

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|

## <a name="remarks"></a>注解

默认情况下，服务器 GC 线程使用各自的 CPU 进行关联，以便为每个处理器提供一个 GC 堆、一个服务器 GC 线程，以及一个后台服务器垃圾回收线程。 从 .NET Framework 4.6.2 开始，当堆数受**GCHeapCount**元素限制时，可以使用**GCHeapAffinitizeMask**元素控制服务器 GC 堆与处理器之间的相关性。

**GCHeapAffinitizeMask**通常与其他两个标志一起使用：

- [GCNoAffinitize](gcnoaffinitize-element.md)，控制服务器 GC 线程/堆是否与 cpu 关联。 `enabled` [GCNoAffinitize](gcnoaffinitize-element.md)元素的属性必须为 `false` 要使用的**GCHeapAffinitizeMask**设置的（其默认值）。

- [GCHeapCount](gcheapcount-element.md)，用于限制进程用于服务器 GC 的堆数。 默认情况下，每个处理器都有一个堆。

**nnnn**是以十进制值表示的位掩码。 字节0的位0表示处理器0，第1个字节表示处理器1，依此类推。 例如：

```xml
<GCHeapAffinitizeMask enabled="1023"/>
```

值1023为0x3FF 或 0011 1111 1111b。 此过程使用10个处理器，从处理器0到处理器9。

## <a name="example"></a>示例

下面的示例指示应用程序使用具有10个堆/线程的服务器 GC。 由于不希望这些堆与系统上运行的其他应用程序中的堆重叠，因此，请使用**GCHeapAffinitizeMask**指定进程应使用 cpu 0 到9。

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCHeapAffinitizeMask enabled="1023"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [GCNoAffinitize 元素](gcnoaffinitize-element.md)
- [GCHeapCount 元素](gcheapcount-element.md)
- [垃圾回收的基本知识](../../../../standard/garbage-collection/fundamentals.md)
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
