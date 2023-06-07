---
description: 了解 C# 中的值类型、种类以及内置值类型
title: 值类型 - C# 参考
ms.date: 01/22/2020
f1_keywords:
- cs.valuetypes
helpviewer_keywords:
- value types [C#]
- types [C#], value types
- C# language, value types
ms.assetid: 471eb994-2958-49d5-a6be-19b4313f80a3
ms.openlocfilehash: 64c9e9eba2495531cfef8a603d53fb21c95c87a4
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599390"
---
# <a name="value-types-c-reference"></a>值类型（C# 参考）

值类型和[引用类型](../keywords/reference-types.md)是 C# 类型的两个主要类别。 值类型的变量包含类型的实例。 它不同于引用类型的变量，后者包含对类型实例的引用。 默认情况下，在[分配](../operators/assignment-operator.md)中，通过将实参传递给方法并返回方法结果来复制变量值。 对于值类型变量，会复制相应的类型实例。 以下示例演示了该行为：

[!code-csharp[copy of values](snippets/shared/ValueTypes.cs#ValueTypeCopied)]

如前面的示例所示，对值类型变量的操作只影响存储在变量中的值类型实例。

如果值类型包含引用类型的数据成员，则在复制值类型实例时，只会复制对引用类型实例的引用。 副本和原始值类型实例都具有对同一引用类型实例的访问权限。 以下示例演示了该行为：

[!code-csharp[shallow copy](snippets/shared/ValueTypes.cs#ShallowCopy)]

> [!NOTE]
> 若要使代码更不易出错、更可靠，请定义并使用不可变的值类型。 本文仅为演示目的使用可变值类型。

## <a name="kinds-of-value-types-and-type-constraints"></a>值类型的种类以及类型约束

值类型可以是以下种类之一：

- [结构类型](struct.md)，用于封装数据和相关功能
- [枚举类型](enum.md)，由一组命名常数定义，表示一个选择或选择组合

[可为 null 值类型](nullable-value-types.md) `T?` 表示其基础值类型 `T` 的所有值及额外的 [null](../keywords/null.md) 值。 不能将 `null` 分配给值类型的变量，除非它是可为 null 的值类型。

你可使用 [`struct` 约束](../../programming-guide/generics/constraints-on-type-parameters.md)指定类型参数为不可为 null 的值类型。 结构类型和枚举类型都满足 `struct` 约束。 从 C# 7.3 开始，你可以在基类约束中使用 `System.Enum`（称为[枚举约束](../../programming-guide/generics/constraints-on-type-parameters.md#enum-constraints)），以指定类型参数为枚举类型。

## <a name="built-in-value-types"></a>内置值类型

C# 提供以下内置值类型，也称为“简单类型”：

- [整型数值类型](integral-numeric-types.md)
- [浮点型数值类型](floating-point-numeric-types.md)
- [bool](bool.md)，表示布尔值
- [char](char.md)，表示 Unicode UTF-16 字符

所有简单值都是结构类型，它们与其他结构类型的不同之处在于，它们允许特定的额外操作：

- 可以使用文字为简单类型提供值。 例如，`'A'` 是类型 `char` 的文本，`2001` 是类型 `int` 的文本。

- 可以使用 [const](../keywords/const.md) 关键字声明简单类型的常数。 不能具有其他结构类型的常数。

- 常数表达式的操作数都是简单类型的常数，在编译时进行评估。

从 C# 7.0 开始，C# 支持[值元组](value-tuples.md)。 值元组是值类型，而不是简单类型。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [值类型](~/_csharplang/spec/types.md#value-types)
- [简单类型](~/_csharplang/spec/types.md#simple-types)
- [变量](~/_csharplang/spec/variables.md)

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- <xref:System.ValueType?displayProperty=nameWithType>
- [引用类型](../keywords/reference-types.md)
