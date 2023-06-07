---
title: 后期绑定重载决策不能应用于“<procedurename>”，因为访问实例是一个接口类型
ms.date: 07/20/2015
f1_keywords:
- vbc30933
- bc30933
helpviewer_keywords:
- overload resolution [Visual Basic], with late-bound argument
- BC30933
ms.assetid: 8182eea0-dd34-4d6e-9ca0-41d8713e9dc4
ms.openlocfilehash: 090ec6f3bbf56350fda2ab15c974b0bc6b15e3d3
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162513"
---
# <a name="bc30933-latebound-overload-resolution-cannot-be-applied-to-procedurename-because-the-accessing-instance-is-an-interface-type"></a>BC30933：后期绑定重载决策不能应用于 " \<procedurename> "，因为访问实例是一个接口类型

编译器尝试解析对重载属性或过程的引用，但引用失败，因为参数的类型为 `Object` ，而引用对象具有接口的数据类型。 `Object`参数强制编译器将引用解析为晚期绑定。

在这些情况下，编译器通过实现类解析重载，而不是通过基础接口解析。 如果类重命名其中一个重载版本，则编译器不会将该版本视为重载，因为其名称不同。 这进而导致编译器忽略重命名的版本，因为它可能是解析引用的正确选择。

**错误 ID：** BC30933

## <a name="to-correct-this-error"></a>更正此错误

- 使用 `CType` 将参数从强制转换 `Object` 为你要调用的重载的签名所指定的类型。

  请注意，它不会帮助将引用对象强制转换为基础接口。 必须强制转换参数以避免此错误。

## <a name="example"></a>示例

下面的示例演示对重载 `Sub` 过程的调用，该过程在编译时导致此错误。

```vb
Module m1
    Interface i1
        Sub s1(ByVal p1 As Integer)
        Sub s1(ByVal p1 As Double)
    End Interface
    Class c1
        Implements i1
        Public Overloads Sub s1(ByVal p1 As Integer) Implements i1.s1
        End Sub
        Public Overloads Sub s2(ByVal p1 As Double) Implements i1.s1
        End Sub
    End Class
    Sub Main()
        Dim refer As i1 = New c1
        Dim o1 As Object = 3.1415
        ' The following reference is INVALID and causes a compiler error.
        refer.s1(o1)
    End Sub
End Module
```

在前面的示例中，如果编译器允许调用编写的 `s1` ，则通过类 `c1` 而不是接口进行解析 `i1` 。 这意味着编译器不会考虑 `s2` ，因为它的名称在中是不同的 `c1` ，即使它是定义的正确选择 `i1` 。

可以通过将调用更改为以下代码行之一来更正此错误：

```vb
refer.s1(CType(o1, Integer))
refer.s1(CType(o1, Double))
```

前面的每个代码行显式将变量强制 `Object` 转换 `o1` 为为重载定义的参数类型之一。

## <a name="see-also"></a>另请参阅

- [过程重载](../../programming-guide/language-features/procedures/procedure-overloading.md)
- [重载决策](../../programming-guide/language-features/procedures/overload-resolution.md)
- [CType Function](../functions/ctype-function.md)
