---
title: 如何：访问对象的成员
ms.date: 07/20/2015
helpviewer_keywords:
- members [Visual Basic], accessing
- object variables [Visual Basic], accessing members
ms.assetid: a0072514-6a79-4dd6-8d03-ca8c13e61ddc
ms.openlocfilehash: 2826a3c98b9f19b08cc943d0f67cdd34ac90f526
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410537"
---
# <a name="how-to-access-members-of-an-object-visual-basic"></a>如何：访问对象的成员 (Visual Basic)

如果对象变量引用某个对象，则通常需要使用该对象的成员，例如其方法、属性、字段和事件。 例如，在创建新的 <xref:System.Windows.Forms.Form> 对象后，你可能需要设置其 <xref:System.Windows.Forms.Control.Text%2A> 属性或调用其 <xref:System.Windows.Forms.Control.Focus%2A> 方法。

## <a name="accessing-members"></a>访问成员

您可以通过引用对象的变量来访问该对象的成员。

#### <a name="to-access-members-of-an-object"></a>访问对象的成员

- 在 `.` 对象变量名称和成员名称之间使用成员访问运算符（）。

    ```vb
    currentText = newForm.Text
    ```

    如果该成员是[共享](../../../language-reference/modifiers/shared.md)的，则不需要使用变量来访问它。

## <a name="accessing-members-of-an-object-of-known-type"></a>访问已知类型的对象成员

如果在编译时知道对象的类型，则可以对引用它的变量使用*早期绑定*。

#### <a name="to-access-members-of-an-object-for-which-you-know-the-type-at-compile-time"></a>访问在编译时知道其类型的对象成员

1. 将对象变量声明为要分配给变量的对象的类型。

    ```vb
    Dim extraForm As System.Windows.Forms.Form
    ```

    使用 `Option Strict On` ，你只能将 <xref:System.Windows.Forms.Form> 对象（或派生自的类型的对象 <xref:System.Windows.Forms.Form> ）分配给 `extraForm` 。 如果您已定义了一个使用扩大转换的类或结构 `CType` <xref:System.Windows.Forms.Form> ，则还可以将该类或结构分配给 `extraForm` 。

2. 在 `.` 对象变量名称和成员名称之间使用成员访问运算符（）。

    ```vb
    extraForm.Show()
    ```

    您可以访问特定于类的所有方法和属性 <xref:System.Windows.Forms.Form> ，无论 `Option Strict` 设置是什么。

## <a name="accessing-members-of-an-object-of-unknown-type"></a>访问类型未知的对象的成员

如果在编译时不知道对象的类型，则必须对引用它的任何变量使用*后期绑定*。

#### <a name="to-access-members-of-an-object-for-which-you-do-not-know-the-type-at-compile-time"></a>访问在编译时不知道其类型的对象成员

1. 将对象变量声明为[对象数据类型](../../../language-reference/data-types/object-data-type.md)。 （将变量声明为与 `Object` 将其声明为相同 <xref:System.Object?displayProperty=nameWithType> 。）

    ```vb
    Dim someControl As Object
    ```

    使用 `Option Strict On` ，你只可以访问在类上定义的成员 <xref:System.Object> 。

2. 在 `.` 对象变量名称和成员名称之间使用成员访问运算符（）。

    ```vb
    someControl.GetType()
    ```

    若要能够访问分配给对象变量的任何对象的成员，必须设置 `Option Strict Off` 。 当你执行此操作时，编译器无法保证给定成员由分配给变量的对象公开。 如果对象未公开尝试访问的成员，则 <xref:System.MemberAccessException> 会出现异常。

## <a name="see-also"></a>另请参阅

- <xref:System.Object>
- <xref:System.Windows.Forms.Form>
- <xref:System.MemberAccessException>
- [对象变量](object-variables.md)
- [对象变量声明](object-variable-declaration.md)
- [Object Data Type](../../../language-reference/data-types/object-data-type.md)
- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
