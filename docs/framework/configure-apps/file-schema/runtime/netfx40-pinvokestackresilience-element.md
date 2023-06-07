---
title: <NetFx40_PInvokeStackResilience> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_PInvokeStackResilience> element
- NetFx40_PInvokeStackResilience element
ms.assetid: 39fb1588-72a4-4479-af74-0605233b68bd
ms.openlocfilehash: 86f50aafe0b21d5080288e09ac7118ca1e4c939a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73116163"
---
# <a name="netfx40_pinvokestackresilience-element"></a>\<NetFx40_PInvokeStackResilience> 元素

指定运行时是否以减慢托管和非托管代码之间的转换速度为代价，在运行时自动修复不正确的平台调用声明。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<NetFx40_PInvokeStackResilience>**  

## <a name="syntax"></a>语法

```xml
<NetFx40_PInvokeStackResilience  enabled="1|0"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br /> 指定运行时是否检测到不正确的平台调用声明，并在运行时在32位平台上自动修复堆栈。|

## <a name="enabled-attribute"></a>enabled 特性

|值|说明|
|-----------|-----------------|
|`0`|运行时使用 .NET Framework 4 中引入的更快互操作封送处理体系结构，该体系结构不会检测并修复不正确的平台调用声明。 这是默认设置。|
|`1`|运行时使用检测并修复不正确的平台调用声明的慢速转换。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关运行时初始化选项的信息。|

## <a name="remarks"></a>注解

此元素使您能够以更快的互操作封送处理为依据不正确的平台调用声明来处理运行时复原。

从 .NET Framework 4 开始，简化的互操作封送处理体系结构可为从托管代码到非托管代码的转换提供显著的性能改进。 在 .NET Framework 的早期版本中，封送层在32位平台上检测到不正确的平台调用声明，并自动修复堆栈。 新的封送处理体系结构消除了此步骤。 因此，转换速度非常快，但不正确的平台调用声明可能导致程序失败。

为了便于在开发期间检测到不正确的声明，Visual Studio 调试体验也得到了改进。 当应用程序在附加调试器中运行时， [pInvokeStackImbalance](../../../debug-trace-profile/pinvokestackimbalance-mda.md)托管调试助手（MDA）将通知您不正确的平台调用声明。

若要解决您的应用程序使用无法重新编译的组件，并且具有不正确的平台调用声明的情况，可以使用 `NetFx40_PInvokeStackResilience` 元素。 将此元素添加到应用程序配置文件中，并将其 `enabled="1"` "导入" 到兼容模式，并且具有较早版本的 .NET Framework 的行为，代价是慢于转换。 已针对早期版本的 .NET Framework 编译的程序集会自动选择进入此兼容模式，并且不需要此元素。

## <a name="configuration-file"></a>配置文件

此元素只能在应用程序配置文件中使用。

## <a name="example"></a>示例

下面的示例演示如何针对应用程序的不正确的平台调用声明提高复原能力，降低了托管和非托管代码之间的转换速度。

```xml
<configuration>
   <runtime>
      <NetFx40_PInvokeStackResilience enabled="1"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [pInvokeStackImbalance](../../../debug-trace-profile/pinvokestackimbalance-mda.md)
