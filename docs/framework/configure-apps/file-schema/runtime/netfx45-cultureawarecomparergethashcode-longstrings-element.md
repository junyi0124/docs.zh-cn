---
title: <NetFx45_CultureAwareComparerGetHashCode_LongStrings> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- NetFx45_CultureAwareComparerGetHashCode_LongStrings element
- <NetFx45_CultureAwareComparerGetHashCode_LongStrings> element
- GetHashCode method
- hash codes, calculating
ms.assetid: 3a5f38d1-ebc8-44de-aaeb-2929f6e6b48f
ms.openlocfilehash: 413eb6c6e61b509135601c65cf045eabd849e8b3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "74802113"
---
# <a name="netfx45_cultureawarecomparergethashcode_longstrings-element"></a>\<NetFx45_CultureAwareComparerGetHashCode_LongStrings> 元素

指定运行时是否使用固定的内存量来计算 <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> 方法的哈希代码。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>**  

## <a name="syntax"></a>语法

```xml
<NetFx45_CultureAwareComparerGetHashCode_LongStrings enabled="0|1">
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`enabled`|必需的特性。<br /><br /> 指定公共语言运行时是否在计算哈希代码时分配固定的内存量。|

## <a name="enabled-attribute"></a>enabled 特性

|值|说明|
|-----------|-----------------|
|0|公共语言运行时为 <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> 方法分配可变的内存量来计算哈希代码。 这是默认设置。|
|1|公共语言运行时为 <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> 方法分配固定的内存量来计算哈希代码。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关运行时初始化选项的信息。|

## <a name="remarks"></a>注解

默认情况下，公共语言运行时将为 <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> 方法分配可变的内存量，当该方法尝试计算非常大的字符串（几百万个字符以上）的哈希代码时，会引发 <xref:System.ArgumentException> 。 通过将此元素添加到应用程序配置文件并将其 `enabled` 特性设置为“1”，你可以指定 <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> 方法使用可分配固定内存量以计算哈希代码的替代算法。

> [!IMPORTANT]
> `<NetFx45_CultureAwareComparerGetHashCode_LongStrings>`Windows 8 及更高版本中不使用元素。

## <a name="see-also"></a>另请参阅

- <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType>
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
