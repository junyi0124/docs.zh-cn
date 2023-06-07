---
title: 无法推断出变量“<variablename>”的类型，因为它绑定到封闭范围中的某个字段
ms.date: 07/20/2015
f1_keywords:
- vbc42110
- bc42110
helpviewer_keywords:
- BC42110
ms.assetid: ef4442eb-08d1-434f-a03b-4aa2ed4e4414
ms.openlocfilehash: 3e76ffea283de2843fc5586179074c01a053ece8
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161278"
---
# <a name="bc42110-the-type-for-variable-variablename-will-not-be-inferred-because-it-is-bound-to-a-field-in-an-enclosing-scope"></a>BC42110：不会推断变量 "" 的类型， \<variablename> 因为它绑定到封闭范围中的某个字段

不会推断变量 "" 的类型， \<variablename> 因为它绑定到封闭范围中的某个字段。 更改 "" 的名称 \<variablename> ，或使用完全限定名称 (例如 "variablename" 或 "variablename" ) 。

代码中的循环控制变量与类的字段或其他封闭范围的名称相同。 因为在没有子句的情况下使用控制变量 `As` ，它将绑定到封闭范围中的字段，并且编译器不会为其创建新的变量，也不会推断其类型。

在下面的示例中， `Index` 语句中的控制变量 `For` 绑定到 `Index` 类中的字段 `Customer` 。 编译器不会为控件变量创建新的变量，也不会 `Index` 推断其类型。

```vb
Class Customer

    ' The class has a field named Index.
    Private Index As Integer

    Sub Main()

    ' The following line will raise this warning.
        For Index = 1 To 10
            ' ...
        Next

    End Sub
End Class
```

默认情况下，此消息是一个警告。 有关如何隐藏警告或如何将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

**错误 ID：** BC42110

## <a name="to-address-this-warning"></a>解决此警告

- 将循环控制变量的名称更改为不属于类的字段名称的标识符，使该循环控制变量成为局部变量。

  ```vb
  For I = 1 To 10
  ```

- 阐明循环控制变量是通过 `Me.` 在变量名称的前面加上前缀来绑定到类字段。

  ```vb
  For Me.Index = 1 To 10
  ```

- 使用 `As` 子句来指定循环控制变量的类型，而不是依赖于局部类型推理。

  ```vb
  For Index As Integer = 1 To 10
  ```

## <a name="example"></a>示例

 下面的代码显示了前面的示例，其中的第一个更正是就地的。

```vb
Class Customer

    ' The class has a field named Index.
    Private Index As Integer

    Sub Main()

        For I = 1 To 10
            ' ...
        Next

    End Sub
End Class
```

## <a name="see-also"></a>另请参阅

- [Option Infer 语句](../statements/option-infer-statement.md)
- [For Each...Next 语句](../statements/for-each-next-statement.md)
- [For...Next 语句](../statements/for-next-statement.md)
- [如何：引用对象的当前实例](../../programming-guide/language-features/variables/how-to-refer-to-the-current-instance-of-an-object.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
- [Me、My、MyBase 和 MyClass](../../programming-guide/program-structure/me-my-mybase-and-myclass.md)
