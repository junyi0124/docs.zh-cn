---
title: <Thread_UseAllCpuGroups> 元素
ms.date: 03/30/2017
ms.assetid: d30fe7c5-8469-46e2-b804-e3eec7b24256
ms.openlocfilehash: a3a612c0ffbcb211157b9623d298ce8ad7a13e94
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73115400"
---
# <a name="thread_useallcpugroups-element"></a>\<Thread_UseAllCpuGroups> 元素

指定运行时是否跨所有 CPU 组分发托管的线程。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<Thread_UseAllCpuGroups>**  

## <a name="syntax"></a>语法

```xml
<Thread_UseAllCpuGroups
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br /> 指定运行时是否跨所有 CPU 组分发托管的线程。|

## <a name="enabled-attribute"></a>enabled 特性

|值|说明|
|-----------|-----------------|
|`false`|运行时不会跨多个 CPU 组分发托管线程。 这是默认设置。|
|`true`|如果计算机具有多个 CPU 组并且元素已启用，则运行时将跨多个 CPU 组分发托管线程 [\<GCCpuGroup>](gccpugroup-element.md) 。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|

## <a name="remarks"></a>注解

如果计算机具有多个 CPU 组，则启用此元素会使运行时将托管线程分散到所有 CPU 组。 若要使用此功能，还必须启用 [\<GCCpuGroup>](gccpugroup-element.md) 元素，该元素将垃圾回收扩展到所有 CPU 组，并在创建和平衡堆时考虑所有核心。 启用 [\<GCCpuGroup>](gccpugroup-element.md) 元素需要启用 [\<gcServer>](gcserver-element.md) 元素。 如果未启用这些元素，则启用 `<Thread_UseAllCpuGroups>` 元素不起作用。

## <a name="example"></a>示例

下面的示例演示如何启用对多个 CPU 组的支持。

```xml
<configuration>
   <runtime>
      <Thread_UseAllCpuGroups enabled="true"/>
      <GCCpuGroup enabled="true"/>
      <gcServer enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [\<GCCpuGroup>Element](gccpugroup-element.md)
