---
title: 枚举类型 - C# 参考
description: 了解表示选项或选项组合的 C# 枚举类型
ms.date: 12/13/2019
f1_keywords:
- enum
- enum_CSharpKeyword
helpviewer_keywords:
- enum keyword [C#]
- enum type [C#]
- enumeration type [C#]
- bit flags [C#]
ms.assetid: bbeb9a0f-e9b3-41ab-b0a6-c41b1a08974c
ms.openlocfilehash: a21bdf63247dc5fec95922de017e1d3502e08565
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599429"
---
# <a name="enumeration-types-c-reference"></a>枚举类型（C# 参考）

枚举类型   是由基础[整型数值类型](value-types.md)的一组命名常量定义的[值类型](integral-numeric-types.md)。 若要定义枚举类型，请使用 `enum` 关键字并指定枚举成员  的名称：

```csharp
enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```

默认情况下，枚举成员的关联常数值为类型 `int`；它们从零开始，并按定义文本顺序递增 1。 可以显式指定任何其他[整数数值](integral-numeric-types.md)类型作为枚举类型的基础类型。 还可以显式指定关联的常数值，如下面的示例所示：

```csharp
enum ErrorCode : ushort
{
    None = 0,
    Unknown = 1,
    ConnectionLost = 100,
    OutlierReading = 200
}
```

不能在枚举类型的定义内定义方法。 若要向枚举类型添加功能，请创建[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)。

枚举类型 `E` 的默认值是由表达式 `(E)0` 生成的值，即使零没有相应的枚举成员也是如此。

可以使用枚举类型，通过一组互斥值或选项组合来表示选项。 若要表示选项组合，请将枚举类型定义为位标志。

## <a name="enumeration-types-as-bit-flags"></a>作为位标志的枚举类型

如果希望枚举类型表示选项组合，请为这些选项定义枚举成员，以便单个选项成为位字段。 也就是说，这些枚举成员的关联值应该是 2 的幂。 然后，可以使用[按位逻辑运算符`|`或 `&`](../operators/bitwise-and-shift-operators.md#enumeration-logical-operators) 分别合并选项或交叉组合选项。 若要指示枚举类型声明位字段，请对其应用 [Flags](xref:System.FlagsAttribute) 属性。 如下面的示例所示，还可以在枚举类型的定义中包含一些典型组合。

[!code-csharp[enum flags](snippets/shared/EnumType.cs#Flags)]

有关详细信息和示例，请参阅 <xref:System.FlagsAttribute?displayProperty=nameWithType> API 参考页和 <xref:System.Enum?displayProperty=nameWithType> API 参考页的[非独占成员和 Flags 属性](/dotnet/api/system.enum#non-exclusive-members-and-the-flags-attribute)部分。

## <a name="the-systemenum-type-and-enum-constraint"></a>System.Enum 类型和枚举约束

<xref:System.Enum?displayProperty=nameWithType> 类型是所有枚举类型的抽象基类。 它提供多种方法来获取有关枚举类型及其值的信息。 有关更多信息和示例，请参阅 <xref:System.Enum?displayProperty=nameWithType> API 参考页。

从 C# 7.3 开始，你可以在基类约束中使用 `System.Enum`（称为[枚举约束](../../programming-guide/generics/constraints-on-type-parameters.md#enum-constraints)），以指定类型参数为枚举类型。 所有枚举类型也都满足 `struct` 约束，此约束用于指定类型参数为不可为 null 的值类型。

## <a name="conversions"></a>转换

对于任何枚举类型，枚举类型与其基础整型类型之间存在显式转换。 如果将枚举值[转换](../operators/type-testing-and-cast.md#cast-expression)为其基础类型，则结果为枚举成员的关联整数值。

[!code-csharp[enum conversions](snippets/shared/EnumType.cs#Conversions)]

使用 <xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> 方法来确定枚举类型是否包含具有特定关联值的枚举成员。

对于任何枚举类型，都存在分别与 <xref:System.Enum?displayProperty=nameWithType> 类型的[装箱和取消装箱](../../programming-guide/types/boxing-and-unboxing.md)相互转换。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [枚举](~/_csharplang/spec/enums.md)
- [枚举值和操作](~/_csharplang/spec/enums.md#enum-values-and-operations)
- [枚举逻辑运算符](~/_csharplang/spec/expressions.md#enumeration-logical-operators)
- [枚举比较运算符](~/_csharplang/spec/expressions.md#enumeration-comparison-operators)
- [显式枚举转换](~/_csharplang/spec/conversions.md#explicit-enumeration-conversions)
- [隐式枚举转换](~/_csharplang/spec/conversions.md#implicit-enumeration-conversions)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [枚举格式字符串](../../../standard/base-types/enumeration-format-strings.md)
- [设计准则 - 枚举设计](../../../standard/design-guidelines/enum.md)
- [设计准则 - 枚举命名约定](../../../standard/design-guidelines/names-of-classes-structs-and-interfaces.md#naming-enumerations)
- [switch 语句](../keywords/switch.md)
