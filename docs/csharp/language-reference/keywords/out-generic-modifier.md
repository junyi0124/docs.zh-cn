---
description: out 关键字（泛型修饰符） - C# 参考
title: out 关键字（泛型修饰符） - C# 参考
ms.date: 07/20/2015
helpviewer_keywords:
- covariance, out keyword [C#]
- out keyword [C#]
ms.assetid: f8c20dec-a8bc-426a-9882-4076b1db1e00
ms.openlocfilehash: 84f3647309c0772f6ae61d3614f8649fe277f153
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89142331"
---
# <a name="out-generic-modifier-c-reference"></a>out（泛型修饰符）（C# 参考）

对于泛型类型参数，`out` 关键字可指定类型参数是协变的。 可以在泛型接口和委托中使用 `out` 关键字。

协变使你使用的类型可以比泛型参数指定的类型派生程度更大。 这样可以隐式转换实现协变接口的类以及隐式转换委托类型。 引用类型支持协变和逆变，但值类型不支持它们。

具有协变类型参数的接口使其方法返回的类型可以比类型参数指定的类型派生程度更大。 例如，因为在 .NET Framework 4 的 <xref:System.Collections.Generic.IEnumerable%601> 中，类型 T 是协变的，所以可以将 `IEnumerable(Of String)` 类型的对象分配给 `IEnumerable(Of Object)` 类型的对象，而无需使用任何特殊转换方法。

可以向协变委托分配相同类型的其他委托，不过要使用派生程度更大的泛型类型参数。

有关详细信息，请参阅[协变和逆变](../../programming-guide/concepts/covariance-contravariance/index.md)。

## <a name="example---covariant-generic-interface"></a>示例 - 协变泛型接口

下面的示例演示如何声明、扩展和实现协变泛型接口。 它还演示如何对实现协变接口的类使用隐式转换。

[!code-csharp[csVarianceKeywords#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csvariancekeywords/cs/program.cs#3)]

在泛型接口中，如果类型参数满足以下条件，则可以声明为协变：

- 类型参数只用作接口方法的返回类型，而不用作方法参数的类型。

    > [!NOTE]
    > 此规则有一个例外。 如果在协变接口中将逆变泛型委托用作方法参数，则可以将协变类型用作此委托的泛型类型参数。 有关协变和逆变泛型委托的详细信息，请参阅[委托中的变体](../../programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)和[对 Func 和 Action 泛型委托使用变体](../../programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)。

- 类型参数不用作接口方法的泛型约束。

## <a name="example---covariant-generic-delegate"></a>示例 - 协变泛型委托

以下示例演示如何声明、实例化和调用协变泛型委托。 它还演示如何隐式转换委托类型。

[!code-csharp[csVarianceKeywords#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csvariancekeywords/cs/program.cs#4)]

在泛型委托中，如果类型仅用作方法返回类型，而不用于方法参数，则可以声明为协变。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [泛型接口中的变体](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [in](in-generic-modifier.md)
- [修饰符](index.md)
