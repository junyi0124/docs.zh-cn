---
title: t 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/gcConcurrent
- http://schemas.microsoft.com/.NetConfiguration/v2.0#gcConcurrent
helpviewer_keywords:
- container tags, <gcConcurrent> element
- gcConcurrent element
- <gcConcurrent> element
ms.assetid: 503f55ba-26ed-45ac-a2ea-caf994da04cd
ms.openlocfilehash: 249518ae7477d284d50f9010757db83b7752c657
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "82102900"
---
# <a name="gcconcurrent-element"></a>\<gcConcurrent> 元素

指定公共语言运行时是否在单独线程上运行垃圾回收。

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<gcConcurrent>

## <a name="syntax"></a>语法

```xml
<gcConcurrent
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br />指定运行时是否并发运行服务器垃圾回收。|

#### <a name="enabled-attribute"></a>enabled 属性

|值|说明|
|-----------|-----------------|
|`false`|不并发运行垃圾回收。|
|`true`|并发运行垃圾回收。 这是默认设置。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|

## <a name="remarks"></a>注解

在 .NET Framework 4 之前，工作站垃圾回收支持并发垃圾回收，这会在单独的线程上以后台执行垃圾回收。 在 .NET Framework 4 中，并发垃圾回收已替换为后台 GC，此操作还在单独的线程上的后台执行垃圾回收。 从 .NET Framework 4.5 开始，后台收集在服务器垃圾回收中变为可用。 **T**元素控制运行时是执行并发还是后台垃圾回收（如果可用），或者是否执行前台垃圾回收。

### <a name="to-disable-background-garbage-collection"></a>禁用后台垃圾回收

> [!WARNING]
> 从 .NET Framework 4 开始，将由后台垃圾回收取代了并发垃圾回收。 在 .NET Framework 文档中，术语 "*并发*" 和 "*背景*" 可互换使用。 若要禁用后台垃圾回收，请使用**t**元素，如本文所述。

默认情况下，运行时使用并发或后台垃圾回收，回收针对延迟进行了优化。 如果应用程序涉及大量用户交互，则通过让并发垃圾回收保持启用状态，可最大限度缩短应用程序执行垃圾回收时的暂停时间。 如果将 `enabled` **t**元素的属性设置为，则 `false` 运行时将使用非并发垃圾回收，这是针对吞吐量进行优化的。

以下配置文件禁用后台垃圾回收：

```xml
<configuration>
   <runtime>
      <gcConcurrent enabled="false"/>
   </runtime>
</configuration>
```

如果计算机配置文件中存在**gcConcurrentSetting**设置，则它将为所有 .NET Framework 应用程序定义默认值。 计算机配置文件设置将重写应用程序配置文件设置。

有关并发和后台垃圾回收的详细信息，请参阅[后台垃圾](../../../../standard/garbage-collection/background-gc.md)回收。

## <a name="example"></a>示例

下面的示例启用后台垃圾回收：

```xml
<configuration>
   <runtime>
      <gcConcurrent enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [垃圾回收基础](../../../../standard/garbage-collection/fundamentals.md)
