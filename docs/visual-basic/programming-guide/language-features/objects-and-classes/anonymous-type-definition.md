---
title: 匿名类型定义
ms.date: 07/20/2015
helpviewer_keywords:
- anonymous types [Visual Basic], type definition
ms.assetid: 7a8a0ddc-55ba-4d67-869e-87a84d938bac
ms.openlocfilehash: 952eb295cc71eab5d0ad6e18f2b697a9b701b434
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404896"
---
# <a name="anonymous-type-definition-visual-basic"></a>匿名类型定义 (Visual Basic)

为了响应匿名类型的实例的声明，编译器会创建一个新的类定义，其中包含类型的指定属性。

## <a name="compiler-generated-code"></a>编译器生成的代码

对于的以下定义 `product` ，编译器将创建包含属性、和的新类定义 `Name` `Price` `OnHand` 。

[!code-vb[VbVbalrAnonymousTypes#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#25)]

类定义包含如下所示的属性定义。 请注意，没有 `Set` 用于键属性的方法。 键属性的值是只读的。

```vb
Public Class $Anonymous1
    Private _name As String
    Private _price As Double
    Private _onHand As Integer
     Public ReadOnly Property Name() As String
        Get
            Return _name
        End Get
    End Property

    Public ReadOnly Property Price() As Double
        Get
            Return _price
        End Get
    End Property

    Public Property OnHand() As Integer
        Get
            Return _onHand
        End Get
        Set(ByVal Value As Integer)
            _onHand = Value
        End Set
    End Property

End Class
```

此外，匿名类型定义包含一个无参数的构造函数。 不允许使用需要参数的构造函数。

如果匿名类型声明至少包含一个键属性，则类型定义将重写继承自的三个成员 <xref:System.Object> ： <xref:System.Object.Equals%2A> 、 <xref:System.Object.GetHashCode%2A> 和 <xref:System.Object.ToString%2A> 。 如果未声明键属性，则只 <xref:System.Object.ToString%2A> 会重写。 替代提供以下功能：

- `Equals``True`如果两个匿名类型实例为同一实例，则返回; 如果满足以下条件，则返回：

  - 它们具有相同数量的属性。

  - 属性以相同顺序声明，具有相同的名称和相同的推断类型。 名称比较不区分大小写。

  - 至少一个属性是键属性，并且 `Key` 关键字应用于相同的属性。

  - 每个对应的键属性对的比较都将返回 `True` 。

    例如，在下面的示例中， `Equals` `True` 仅为 `employee01` 和返回 `employee08` 。 每行前的注释指定新实例不匹配的原因 `employee01` 。

    [!code-vb[VbVbalrAnonymousTypes#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#24)]

- `GetHashcode`提供适当唯一的 GetHashCode 算法。 算法只使用键属性来计算哈希代码。

- `ToString`返回连接的属性值的字符串，如下面的示例中所示。 同时包含键属性和非键属性。

  [!code-vb[VbVbalrAnonymousTypes#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#29)]

匿名类型的显式命名属性不能与这些生成的方法冲突。 也就是说，不能使用 `.Equals` 、 `.GetHashCode` 或 `.ToString` 对属性命名。

包含至少一个键属性的匿名类型定义还实现 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口，其中 `T` 是匿名类型的类型。

> [!NOTE]
> 仅当匿名类型声明出现在同一程序集中时，才创建相同的匿名类型，其属性具有相同的名称和相同的推断类型，属性以相同顺序声明，相同的属性被标记为键属性。

## <a name="see-also"></a>另请参阅

- [匿名类型](anonymous-types.md)
- [如何：推断匿名类型声明中的属性名和类型](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
