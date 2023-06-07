---
title: 强制转换和类型转换 - C# 编程指南
description: 了解强制转换和类型转换，例如隐式、显式（强制转换）和用户定义的转换。
ms.date: 07/06/2020
helpviewer_keywords:
- type conversion [C#]
- data type conversion [C#]
- numeric conversions [C#]
- conversions [C#], type
- casting [C#]
- converting types [C#]
ms.assetid: 568df58a-d292-4b55-93ba-601578722878
ms.openlocfilehash: cfe5376675808559f4bf9c9cd8b21180dcd0cc49
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555323"
---
# <a name="casting-and-type-conversions-c-programming-guide"></a>强制转换和类型转换（C# 编程指南）

由于 C# 是在编译时静态类型化的，因此变量在声明后就无法再次声明，或无法分配另一种类型的值，除非该类型可以隐式转换为变量的类型。 例如，`string` 无法隐式转换为 `int`。 因此，在将 `i` 声明为 `int` 后，无法将字符串“Hello”分配给它，如以下代码所示：

```csharp
int i;

// error CS0029: Cannot implicitly convert type 'string' to 'int'
i = "Hello";
```

但有时可能需要将值复制到其他类型的变量或方法参数中。 例如，可能需要将一个整数变量传递给参数类型化为 `double` 的方法。 或者可能需要将类变量分配给接口类型的变量。 这些类型的操作称为类型转换。 在 C# 中，可以执行以下几种类型的转换：

- **隐式转换**：由于这种转换始终会成功且不会导致数据丢失，因此无需使用任何特殊语法。 示例包括从较小整数类型到较大整数类型的转换以及从派生类到基类的转换。

- **显式转换（强制转换）** ：必须使用[强制转换表达式](../../language-reference/operators/type-testing-and-cast.md#cast-expression)，才能执行显式转换。 在转换中可能丢失信息时或在出于其他原因转换可能不成功时，必须进行强制转换。 典型的示例包括从数值到精度较低或范围较小的类型的转换和从基类实例到派生类的转换。

- **用户定义的转换**：用户定义的转换是使用特殊方法执行，这些方法可定义为在没有基类和派生类关系的自定义类型之间启用显式转换和隐式转换。 有关详细信息，请参阅[用户定义转换运算符](../../language-reference/operators/user-defined-conversion-operators.md)。

- **使用帮助程序类进行转换**：若要在非兼容类型（如整数和 <xref:System.DateTime?displayProperty=nameWithType> 对象，或十六进制字符串和字节数组）之间转换，可使用 <xref:System.BitConverter?displayProperty=nameWithType> 类、<xref:System.Convert?displayProperty=nameWithType> 类和内置数值类型的 `Parse` 方法（如 <xref:System.Int32.Parse%2A?displayProperty=nameWithType>）。 有关详细信息，请参见[如何将字节数组转换为 int](./how-to-convert-a-byte-array-to-an-int.md)、[如何将字符串转换为数字](./how-to-convert-a-string-to-a-number.md)和[如何在十六进制字符串与数值类型之间转换](./how-to-convert-between-hexadecimal-strings-and-numeric-types.md)。

## <a name="implicit-conversions"></a>隐式转换

对于内置数值类型，如果要存储的值无需截断或四舍五入即可适应变量，则可以进行隐式转换。 对于整型类型，这意味着源类型的范围是目标类型范围的正确子集。 例如，[long](../../language-reference/builtin-types/integral-numeric-types.md) 类型的变量（64 位整数）能够存储 [int](../../language-reference/builtin-types/integral-numeric-types.md)（32 位整数）可存储的任何值。 在下面的示例中，编译器先将右侧的 `num` 值隐式转换为 `long` 类型，再将它赋给 `bigNum`。

[!code-csharp[csProgGuideTypes#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#34)]

有关所有隐式数值转换的完整列表，请参阅[内置数值转换](../../language-reference/builtin-types/numeric-conversions.md)一文的[隐式数值转换表](../../language-reference/builtin-types/numeric-conversions.md#implicit-numeric-conversions)部分。

对于引用类型，隐式转换始终存在于从一个类转换为该类的任何一个直接或间接的基类或接口的情况。 由于派生类始终包含基类的所有成员，因此不必使用任何特殊语法。

```csharp
Derived d = new Derived();

// Always OK.
Base b = d;
```

## <a name="explicit-conversions"></a>显式转换

但是，如果进行转换可能会导致信息丢失，则编译器会要求执行显式转换，显式转换也称为强制转换。 强制转换是显式告知编译器以下信息的一种方式：你打算进行转换且你知道可能会发生数据丢失，或者你知道强制转换有可能在运行时失败。 若要执行强制转换，请在要转换的值或变量前面的括号中指定要强制转换到的类型。 下面的程序将 [double](../../language-reference/builtin-types/floating-point-numeric-types.md) 强制转换为 [int](../../language-reference/builtin-types/integral-numeric-types.md)。如不强制转换则该程序不会进行编译。

[!code-csharp[csProgGuideTypes#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#2)]

有关支持的显式数值转换的完整列表，请参阅[内置数值转换](../../language-reference/builtin-types/numeric-conversions.md)一文的[显式数值转换](../../language-reference/builtin-types/numeric-conversions.md#explicit-numeric-conversions)部分。

对于引用类型，如果需要从基类型转换为派生类型，则必须进行显式强制转换：

```csharp
// Create a new derived type.
Giraffe g = new Giraffe();

// Implicit conversion to base type is safe.
Animal a = g;

// Explicit conversion is required to cast back
// to derived type. Note: This will compile but will
// throw an exception at run time if the right-side
// object is not in fact a Giraffe.
Giraffe g2 = (Giraffe)a;
```

引用类型之间的强制转换操作不会更改基础对象的运行时类型；它只更改用作对该对象引用的值的类型。 有关详细信息，请参阅[多态性](../classes-and-structs/polymorphism.md)。

## <a name="type-conversion-exceptions-at-run-time"></a>运行时的类型转换异常

在某些引用类型转换中，编译器无法确定强制转换是否会有效。 正确进行编译的强制转换操作有可能在运行时失败。 如下面的示例所示，类型转换在运行时失败将导致引发 <xref:System.InvalidCastException>。

[!code-csharp[csProgGuideTypes#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#41)]

`Test` 方法有一个 `Animal` 形式参数，因此，将实际参数 `a` 显式强制转换为 `Reptile` 会造成危险的假设。 更安全的做法是不要做出假设，而是检查类型。 C# 提供 [is](../../language-reference/operators/type-testing-and-cast.md#is-operator) 运算符，使你可以在实际执行强制转换之前测试兼容性。 有关详细信息，请参阅[如何使用模式匹配以及 as 和 is 运算符安全地进行强制转换](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[转换](~/_csharplang/spec/conversions.md)部分。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类型](./index.md)
- [强制转换表达式](../../language-reference/operators/type-testing-and-cast.md#cast-expression)
- [用户定义转换运算符](../../language-reference/operators/user-defined-conversion-operators.md)
- [通用类型转换](/previous-versions/visualstudio/visual-studio-2013/yy580hbd(v=vs.120))
- [如何将字符串转换为数字](./how-to-convert-a-string-to-a-number.md)
