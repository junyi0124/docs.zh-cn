---
title: GCNoAffinitize 元素
ms.date: 11/08/2019
helpviewer_keywords:
- gcNoAffinitize element
- <gcNoAffinitize> element
ms.openlocfilehash: 16d6e5adefe2b632d7251669650058d7df7cea70
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "84004733"
---
# <a name="gcnoaffinitize-element"></a>\<GCNoAffinitize> 元素

指定是否关联服务器 GC 线程与 Cpu。

\<configuration>\
&nbsp;&nbsp;\<runtime>\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCNoAffinitize>

## <a name="syntax"></a>语法

```xml
<GCNoAffinitize
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br />指定服务器 GC 线程/堆是否与计算机上可用的处理器关联。|

#### <a name="enabled-attribute"></a>enabled 属性

|值|说明|
|-----------|-----------------|
|`false`|包含 Cpu 的关联服务器 GC 线程。 这是默认设置。|
|`true`|不关联具有 Cpu 的服务器垃圾回收线程。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|

## <a name="remarks"></a>注解

默认情况下，服务器 GC 线程使用各自的 Cpu 进行关联。 系统的每个可用处理器都有其自己的 GC 堆和线程。 这通常是首选的设置，因为它会优化缓存使用量。 从 .NET Framework 4.6.2 开始，通过将**GCNoAffinitize**元素的 `enabled` 属性设置为 `true` ，可以指定服务器 GC 线程和 cpu 不应紧耦合。

您可以单独指定**GCNoAffinitize**配置元素，以不关联服务器 GC 线程与 cpu 一起工作。 你还可以将它与[GCHeapCount](gcheapcount-element.md)元素结合使用，以控制应用程序使用的垃圾回收堆和线程的数目。

如果 `enabled` **GCNoAffinitize**元素的属性是 `false` （其默认值），则还可以使用[GCHeapCount](gcheapcount-element.md)元素来指定垃圾回收器线程和堆的数目，并使用[GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)元素来指定 gc 线程和堆关联的处理器。

## <a name="example"></a>示例

以下示例不关联服务器 GC 线程：

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

以下示例不关联服务器 GC 线程，并将 GC 堆/线程数限制为10：

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
- [GCHeapAffinitizeMask 元素](gcheapaffinitizemask-element.md)
- [GCHeapCount 元素](gcheapcount-element.md)
- [垃圾回收的基本知识](../../../../standard/garbage-collection/fundamentals.md)
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
