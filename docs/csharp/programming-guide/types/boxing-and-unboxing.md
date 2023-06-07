---
title: 装箱和取消装箱 - C# 编程指南
description: 了解 C# 编程中的装箱和取消装箱。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
f1_keywords:
- cs.boxing
helpviewer_keywords:
- C# language, boxing
- C# language, unboxing
- unboxing [C#]
- boxing [C#]
ms.assetid: 8da9bbf4-bce9-4b08-b2e5-f64c11c56514
ms.openlocfilehash: 5a5bfcc79de8ba3ff66ca8aab9d86d69d89f9221
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380691"
---
# <a name="boxing-and-unboxing-c-programming-guide"></a>装箱和取消装箱（C# 编程指南）

装箱是将[值类型](../../language-reference/builtin-types/value-types.md)转换为 `object` 类型或由此值类型实现的任何接口类型的过程。 常见语言运行时 (CLR) 对值类型进行装箱时，会将值包装在 <xref:System.Object?displayProperty=nameWithType> 实例中并将其存储在托管堆中。 取消装箱将从对象中提取值类型。 装箱是隐式的；取消装箱是显式的。 装箱和取消装箱的概念是类型系统 C# 统一视图的基础，其中任一类型的值都被视为一个对象。

下例将整型变量 `i` 进行了装箱并分配给对象 `o`。

[!code-csharp[csProgGuideTypes#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#14)]

然后，可以将对象 `o` 取消装箱并分配给整型变量 `i`：

[!code-csharp[csProgGuideTypes#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#15)]

以下示例演示如何在 C# 中使用装箱。

[!code-csharp[csProgGuideTypes#47](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#47)]

## <a name="performance"></a>性能

相对于简单的赋值而言，装箱和取消装箱过程需要进行大量的计算。 对值类型进行装箱时，必须分配并构造一个新对象。 取消装箱所需的强制转换也需要进行大量的计算，只是程度较轻。 有关更多信息，请参阅[性能](../../../framework/performance/performance-tips.md)。

## <a name="boxing"></a>装箱

装箱用于在垃圾回收堆中存储值类型。 装箱是[值类型](../../language-reference/builtin-types/value-types.md)到 `object` 类型或到此值类型所实现的任何接口类型的隐式转换。 对值类型装箱会在堆中分配一个对象实例，并将该值复制到新的对象中。

请看以下值类型变量的声明：

[!code-csharp[csProgGuideTypes#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#17)]

以下语句对变量 `i` 隐式应用了装箱操作：

[!code-csharp[csProgGuideTypes#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#18)]

此语句的结果是在堆栈上创建对象引用 `o`，而在堆上则引用 `int` 类型的值。 该值是赋给变量 `i` 的值类型值的一个副本。 以下装箱转换图说明了 `i` 和 `o` 这两个变量之间的差异：

![显示 i 和 o 变量之间的差异的图。](./media/boxing-and-unboxing/boxing-operation-i-o-variables.gif)

还可以像下面的示例一样执行显式装箱，但显式装箱从来不是必需的：

[!code-csharp[csProgGuideTypes#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#19)]

## <a name="description"></a>描述

此示例使用装箱将整型变量 `i` 转换为对象 `o`。 这样一来，存储在变量 `i` 中的值就从 `123` 更改为 `456`。 该示例表明原始值类型和装箱的对象使用不同的内存位置，因此能够存储不同的值。

## <a name="example"></a>示例

[!code-csharp[csProgGuideTypes#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#16)]

## <a name="unboxing"></a>取消装箱

取消装箱是从 `object` 类型到[值类型](../../language-reference/builtin-types/value-types.md)或从接口类型到实现该接口的值类型的显式转换。 取消装箱操作包括：

- 检查对象实例，以确保它是给定值类型的装箱值。

- 将该值从实例复制到值类型变量中。

下面的语句演示装箱和取消装箱两种操作：

[!code-csharp[csProgGuideTypes#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#21)]

下图演示了上述语句的结果：

![显示取消装箱转换的图。](./media/boxing-and-unboxing/unboxing-conversion-operation.gif)

要在运行时成功取消装箱值类型，被取消装箱的项必须是对一个对象的引用，该对象是先前通过装箱该值类型的实例创建的。 尝试取消装箱 `null` 会导致 <xref:System.NullReferenceException>。 尝试取消装箱对不兼容值类型的引用会导致 <xref:System.InvalidCastException>。

## <a name="example"></a>示例

下面的示例演示无效的取消装箱及引发的 `InvalidCastException`。 使用 `try` 和 `catch`，在发生错误时显示错误信息。

[!code-csharp[csProgGuideTypes#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#20)]

此程序输出：

`Specified cast is not valid. Error: Incorrect unboxing.`

如果将下列语句：

```csharp
int j = (short) o;
```

更改为：

```csharp
int j = (int) o;
```

将执行转换，并将得到以下输出：

`Unboxing OK.`

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [引用类型](../../language-reference/keywords/reference-types.md)
- [值类型](../../language-reference/builtin-types/value-types.md)
