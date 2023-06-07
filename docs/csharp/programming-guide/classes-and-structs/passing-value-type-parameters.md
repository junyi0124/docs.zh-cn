---
title: 传递值类型参数 - C# 编程指南
description: 将值类型变量传递给 C# 中的值时，任何更改都不会影响原始数据。 若要更改值，请通过引用传递。
ms.date: 07/20/2015
helpviewer_keywords:
- method parameters [C#], value types
- parameters [C#], value
ms.assetid: 193ab86f-5f9b-4359-ac29-7cdf8afad3a6
ms.openlocfilehash: ccc97e2f6e8c6b2b8fd15206a8bcd5cd8a5a8431
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186063"
---
# <a name="passing-value-type-parameters-c-programming-guide"></a>传递值类型参数（C# 编程指南）

一个[值类型](../../language-reference/builtin-types/value-types.md)变量包含它直接与[引用类型](../../language-reference/keywords/reference-types.md)变量相对的数据，其中包含对其数据的引用。 按值将值-类型变量传递给方法，意味着将变量副本传递给该方法。 在方法内发生的对该实参进行的任何更改都不会影响存储在形参变量中的原始数据。 若要使用已调用的方法来更改参数值，必须使用 [ref](../../language-reference/keywords/ref.md) 或 [out](../../language-reference/keywords/out-parameter-modifier.md) 关键字通过引用传递它。 还可以使用 [in](../../language-reference/keywords/in-parameter-modifier.md) 关键字来按引用传递值参数，以避免复制并同时保证不更改值。 为简单起见，下面的示例使用 `ref`。  
  
## <a name="passing-value-types-by-value"></a>按值传递值类型  

 下面的示例演示按值传递值-类型参数。 变量 `n` 按值传递给方法 `SquareIt`。 在方法内发生的任何更改都不会影响该变量的原始值。  
  
 [!code-csharp[csProgGuideParameters#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#3)]  
  
 变量 `n` 是值类型。 它包含其数据，值为 `5`。 调用 `SquareIt` 时，`n` 的内容复制到参数 `x` 中，这是该方法内的平方值。 但是在 `Main` 中，`n` 的值在调用 `SquareIt` 方法之后与之前相同。 在方法内发生的更改只影响本地变量 `x`。  
  
## <a name="passing-value-types-by-reference"></a>按引用传递值类型  

 下面的示例与上述示例相同，除了自变量是作为 `ref` 参数传递的。 `x` 在方法中更改时，基础自变量的值 `n` 也更改。  
  
 [!code-csharp[csProgGuideParameters#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#4)]  
  
 在此示例中，它传递的不是 `n` 的值；而是传递 `n` 的引用。 参数 `x` 不是 [int](../../language-reference/builtin-types/integral-numeric-types.md)；它是对 `int` 的引用，在这种情况下，是对 `n` 的引用。 因此，当 `x` 在方法中进行平方计算时，实际求平方值的就是 `x` 所指的 `n`。  
  
## <a name="swapping-value-types"></a>交换值类型  

 更改自变量值的一个常见示例是交换方法，其中将两个变量传递给方法，并由该方法交换其内容。 必须通过引用将自变量传递给交换方法。 否则，交换参数在方法中的本地副本，并且调用方法中不会发生更改。 下面的示例交换整数值。  
  
 [!code-csharp[csProgGuideParameters#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#5)]  
  
 调用 `SwapByRef` 方法时，在调用中使用 `ref` 关键字，如下面的示例中所示。  
  
 [!code-csharp[csProgGuideParameters#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#6)]  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [传递参数](./passing-parameters.md)
- [传递引用类型参数](./passing-reference-type-parameters.md)
