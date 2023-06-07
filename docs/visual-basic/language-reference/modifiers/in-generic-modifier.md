---
title: In （泛型修饰符）-Visual Basic
ms.date: 07/20/2015
f1_keywords:
- vb.VarianceIn
helpviewer_keywords:
- contravariance, In keyword [Visual Basic]
- In keyword [Visual Basic]
ms.assetid: 59bb13c5-fe96-42b8-8286-86293d1661c5
ms.openlocfilehash: 9c0f7d454767112e1e309af81407b5fdef22eee9
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004873"
---
# <a name="in-generic-modifier-visual-basic"></a>In（泛型修饰符）(Visual Basic)

对于泛型类型参数，`In` 关键字可指定类型参数是逆变的。

## <a name="remarks"></a>备注

逆变使你使用的类型可以比泛型参数指定的类型派生程度更小。 这样可以隐式转换实现变体接口的类以及隐式转换委托类型。

有关详细信息，请参阅[协变和逆变](../../programming-guide/concepts/covariance-contravariance/index.md)。

## <a name="rules"></a>规则

可以在泛型接口和委托中使用 `In` 关键字。
  
如果类型参数仅用作方法参数的类型并且不用作方法返回类型，则可以在泛型接口或委托中声明为逆变。 `ByRef` 参数不能是协变或逆变。

引用类型支持协变和逆变，但值类型不支持协变和逆变。

在 Visual Basic 中，不能声明逆变接口中的事件，而无需指定委托类型。 此外，逆变接口不能有嵌套的类、枚举或结构，但它们可以有嵌套的接口。

## <a name="behavior"></a>行为

具有逆变类型参数的接口使其方法接受的参数的类型可以比接口类型参数指定的类型派生程度更小。 例如，由于在 .NET Framework 4 的 <xref:System.Collections.Generic.IComparer%601> 接口中，类型 T 是逆变的，因此，如果 `Employee` 从 `Person`继承，则可以将 `IComparer(Of Person)` 类型的对象分配给 `IComparer(Of Employee)` 类型的对象，而无需使用任何特殊转换方法。

可以向逆变委托分配相同类型的其他委托，不过要使用派生程度更小的泛型类型参数。

## <a name="example---contravariant-generic-interface"></a>示例-逆变泛型接口

下面的示例演示如何声明、扩展和实现逆变泛型接口。 它还演示如何对实现此接口的类使用隐式转换。

[!code-vb[vbVarianceKeywords#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#1)]

## <a name="example---contravariant-generic-delegate"></a>示例-逆变泛型委托

以下示例演示如何声明、实例化和调用逆变泛型委托。 它还演示如何隐式转换委托类型。

[!code-vb[vbVarianceKeywords#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvariancekeywords/vb/module1.vb#2)]

## <a name="see-also"></a>另请参阅

- [泛型接口中的变体](../../programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)
- [Out](out-generic-modifier.md)
