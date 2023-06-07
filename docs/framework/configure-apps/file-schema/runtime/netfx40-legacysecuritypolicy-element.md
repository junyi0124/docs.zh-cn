---
title: <NetFx40_LegacySecurityPolicy> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_LegacySecurityPolicy> element
- NetFx40_LegacySecurityPolicy element
ms.assetid: 07132b9c-4a72-4710-99d7-e702405e02d4
ms.openlocfilehash: d5192eb56bb8b640544bdc52a0bb9d8a5277efef
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73116255"
---
# <a name="netfx40_legacysecuritypolicy-element"></a>\<NetFx40_LegacySecurityPolicy> 元素

指定运行时是否使用旧版代码访问安全性 (CAS) 策略。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<NetFx40_LegacySecurityPolicy>**  

## <a name="syntax"></a>语法

```xml
<NetFx40_LegacySecurityPolicy
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br /> 指定运行时是否使用旧版 CAS 策略。|

## <a name="enabled-attribute"></a>enabled 特性

|值|说明|
|-----------|-----------------|
|`false`|运行时不使用旧的 CAS 策略。 这是默认设置。|
|`true`|运行时使用旧版 CAS 策略。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关运行时初始化选项的信息。|

## <a name="remarks"></a>注解

在 .NET Framework 版本3.5 及更低版本中，CAS 策略始终有效。 在 .NET Framework 4 中，必须启用 CAS 策略。

CA 策略是特定于版本的。 .NET Framework 早期版本中存在的自定义 CAS 策略必须在 .NET Framework 4 中 respecified。

将 `<NetFx40_LegacySecurityPolicy>` 元素应用到 .NET Framework 4 程序集并不会影响[安全透明的代码](../../../misc/security-transparent-code.md); 透明规则仍适用。

> [!IMPORTANT]
> `<NetFx40_LegacySecurityPolicy>`对于未安装在[全局程序集缓存](../../../app-domains/gac.md)中的[本机映像生成器（ngen.exe）](../../../tools/ngen-exe-native-image-generator.md)创建的本机映像程序集，应用元素可能会导致严重的性能下降。 性能下降是由运行时在应用属性时无法将程序集加载为本机映像而导致其作为实时程序集加载的。

> [!NOTE]
> 如果指定的目标 .NET Framework 版本早于 Visual Studio 项目的项目设置中的 .NET Framework 4，则将启用 CAS 策略，包括你为该版本指定的任何自定义 CAS 策略。 但是，您将不能使用新 .NET Framework 4 类型和成员。 你还可以使用[应用程序配置文件](../../index.md)中的 "启动设置" 架构中的[ \<supportedRuntime> 元素](../startup/supportedruntime-element.md)指定 .NET Framework 的早期版本。

> [!NOTE]
> 配置文件语法区分大小写。 应使用语法和示例部分中提供的语法。

## <a name="configuration-file"></a>配置文件

此元素只能在应用程序配置文件中使用。

## <a name="example"></a>示例

以下示例演示了如何为应用程序启用旧的 CAS 策略。

```xml
<configuration>
   <runtime>
      <NetFx40_LegacySecurityPolicy enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
