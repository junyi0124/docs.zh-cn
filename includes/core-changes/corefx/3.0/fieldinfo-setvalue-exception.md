---
ms.openlocfilehash: 02c9305a36f47dfaf0b1fa8d19b07cd2d34badae
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031977"
---
### <a name="fieldinfosetvalue-throws-exception-for-static-init-only-fields"></a>FieldInfo.SetValue 将对静态、仅初始化字段引发异常

从 .NET Core 3.0 开始，当你尝试通过调用 <xref:System.Reflection.FieldInfo.SetValue%2A?displayProperty=fullName> 在静态 <xref:System.Reflection.FieldAttributes.InitOnly> 字段上设置值时，将引发异常。

#### <a name="change-description"></a>更改描述

在 .NET Framework 和 3.0 之前的 .NET Core 版本中，你可以通过调用 <xref:System.Reflection.FieldInfo.SetValue%2A?displayProperty=fullName> 来设置初始化后为常量的静态字段的值（[C# 中的 readonly](~/docs/csharp/language-reference/keywords/readonly.md)）。 但是，以这种方式设置此类字段导致了不可预测的行为，具体取决于目标框架和优化设置。

在 .NET Core 3.0 及更高版本中，当对静态 <xref:System.Reflection.FieldAttributes.InitOnly> 字段调用 <xref:System.Reflection.FieldInfo.SetValue%2A> 时，将引发 <xref:System.FieldAccessException?displayProperty=nameWithType> 异常。

> [!TIP]
> <xref:System.Reflection.FieldAttributes.InitOnly> 字段是只能在声明它时或位于包含类的构造函数中时设置的字段。 换句话说，它在初始化后为常量。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

初始化静态构造函数中的静态 <xref:System.Reflection.FieldAttributes.InitOnly> 字段。 这同时适用于动态和非动态类型。

或者，可以从字段中删除 <xref:System.Reflection.FieldAttributes.InitOnly?displayProperty=nameWithType> 属性，然后调用 <xref:System.Reflection.FieldInfo.SetValue%2A?displayProperty=nameWithType>。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Reflection.FieldInfo.SetValue(System.Object,System.Object)?displayProperty=nameWithType>
- <xref:System.Reflection.FieldInfo.SetValue(System.Object,System.Object,System.Reflection.BindingFlags,System.Reflection.Binder,System.Globalization.CultureInfo)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Reflection.FieldInfo.SetValue(System.Object,System.Object)`
- `M:System.Reflection.FieldInfo.SetValue(System.Object,System.Object,System.Reflection.BindingFlags,System.Reflection.Binder,System.Globalization.CultureInfo)`

-->
