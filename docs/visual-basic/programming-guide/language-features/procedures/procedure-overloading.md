---
title: 过程重载
ms.date: 07/20/2015
helpviewer_keywords:
- signatures
- Overloads keyword [Visual Basic]
- hiding by signature
- Visual Basic code, procedures
- procedures [Visual Basic], signatures for
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
- parameters [Visual Basic], lists
- signatures [Visual Basic], procedure
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- Shadows keyword [Visual Basic]
- procedure overloading
- procedures [Visual Basic], parameter lists
ms.assetid: fbc7fb18-e3b2-48b6-b554-64c00ed09d2a
ms.openlocfilehash: f8accc74fbdd9b1d8cf9bc3d8f6ddd26f73452b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363871"
---
# <a name="procedure-overloading-visual-basic"></a>过程重载 (Visual Basic)

*重载*过程意味着使用相同的名称但不同的参数列表在多个版本中定义它。 重载的目的是定义过程中的多个紧密相关的版本，而不必按名称对其进行区分。 可以通过改变参数列表来实现此目的。

## <a name="overloading-rules"></a>重载规则

重载过程时，以下规则适用：

- **相同的名称**。 每个重载版本必须使用相同的过程名称。

- **不同的签名**。 每个重载版本必须与所有其他重载版本都有以下两个方面中的至少一个：

  - 参数数目

  - 参数的顺序

  - 参数的数据类型

  - 类型参数的数目（针对泛型过程）

  - 返回类型（仅适用于转换运算符）

  与过程名称一起，以上各项统称为过程的*签名*。 调用重载过程时，编译器将使用签名来检查调用是否与定义正确匹配。

- **项不是签名的一部分**。 不能重载过程而不会改变签名。 具体而言，你不能通过仅改变一个或多个以下项来重载过程：

  - 过程修饰符关键字，如 `Public` 、 `Shared` 和`Static`

  - 参数或类型参数名称

  - 类型参数约束（针对泛型过程）

  - 参数修饰符关键字，如 `ByRef` 和`Optional`

  - 是否返回值

  - 返回值的数据类型（转换运算符除外）

  前面列表中的项不是签名的一部分。 尽管不能使用它们来区分重载版本，但你可以根据其签名正确区分的重载版本来区分它们。

- **后期绑定参数**。 如果打算将后期绑定对象变量传递到重载版本，则必须将相应的参数声明为 <xref:System.Object> 。

## <a name="multiple-versions-of-a-procedure"></a>一个过程的多个版本

假设您正在编写一个 `Sub` 过程，以便在客户的余额中发布事务，并且您希望能够按名称或帐号来引用客户。 为此，可以定义两个不同的 `Sub` 过程，如以下示例中所示：

[!code-vb[VbVbcnProcedures#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#73)]

### <a name="overloaded-versions"></a>重载版本

一种替代方法是重载单个过程名称。 您可以使用[Overloads](../../../language-reference/modifiers/overloads.md)关键字为每个参数列表定义过程的版本，如下所示：

[!code-vb[VbVbcnProcedures#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#72)]

#### <a name="additional-overloads"></a>其他重载

如果还想要在或中接受事务量 `Decimal` ，可以 `Single` 进一步重载 `post` 以允许进行此变体。 如果对上述示例中的每个重载执行了此操作，则会有四个 `Sub` 过程，它们都具有相同的名称，但具有四个不同的签名。

## <a name="advantages-of-overloading"></a>重载的优点

重载过程的优点是调用的灵活性。 若要使用 `post` 前面的示例中声明的过程，调用代码可以获取客户标识作为 `String` 或 `Integer` ，然后在这两种情况下调用相同的过程。 以下示例对此进行了说明：

[!code-vb[VbVbcnProcedures#56](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#56)]

[!code-vb[VbVbcnProcedures#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#57)]

## <a name="see-also"></a>另请参阅

- [过程](./index.md)
- [如何：定义一个过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)
- [如何：调用重载过程](./how-to-call-an-overloaded-procedure.md)
- [如何：重载带有可选参数的过程](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [如何：重载参数数量不确定的过程](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [重载过程注意事项](./considerations-in-overloading-procedures.md)
- [重载决策](./overload-resolution.md)
- [重载](../../../language-reference/modifiers/overloads.md)
- [Generic Types in Visual Basic](../data-types/generic-types.md)
