---
title: AttributeUsage
ms.date: 07/20/2015
ms.assetid: 48757216-c21d-4051-86d5-8a3e03c39d2c
ms.openlocfilehash: 677d49aba38801f2adf42cc745983af30b3eddc5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400727"
---
# <a name="attributeusage-visual-basic"></a>AttributeUsage （Visual Basic）

确定如何使用自定义特性类。 `AttributeUsage` 是一个特性，可应用于自定义特性定义，以控制如何应用新特性。 在显式应用时的默认设置如下：

```vb
<System.AttributeUsage(System.AttributeTargets.All,
                   AllowMultiple:=False,
                   Inherited:=True)>
Class NewAttribute
    Inherits System.Attribute
End Class
```

在此示例中，`NewAttribute` 类可应用于任何特性的代码实体，但只能对每个实体应用一次。 当应用于基类时，它由派生类继承。

`AllowMultiple` 和 `Inherited` 参数是可选的，因而此代码具有相同的效果：

```vb
<System.AttributeUsage(System.AttributeTargets.All)>
Class NewAttribute
    Inherits System.Attribute
End Class
```

第一个 `AttributeUsage` 参数必须是 <xref:System.AttributeTargets> 枚举的一个或多个元素。 可将多个目标类型与 OR 运算符链接在一起，如下所示：

```vb
<AttributeUsage(AttributeTargets.Property Or AttributeTargets.Field)>
Class NewPropertyOrFieldAttribute
    Inherits Attribute
End Class
```

如果 `AllowMultiple` 参数设置为 `true`，则结果特性可多次应用于单个实体，如下所示：

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)>
Class MultiUseAttr
    Inherits Attribute
End Class

<MultiUseAttr(), MultiUseAttr()>
Class Class1
End Class
```

在本例中，`MultiUseAttr` 可重复应用，因为 `AllowMultiple` 设置为 `true`。 所显示的两种用于应用多个特性的格式均有效。

如果 `Inherited` 设置为 `false`，那么特性不会由派生自已特性化的类的类继承。 例如：

```vb
<AttributeUsage(AttributeTargets.Class, Inherited:=False)>
Class Attr1
    Inherits Attribute
End Class

<Attr1()>
Class BClass

End Class

Class DClass
    Inherits BClass
End Class
```

在本例中，`Attr1` 不会通过继承应用于 `DClass`。

## <a name="remarks"></a>备注

`AttributeUsage` 特性是单次使用的特性 -- 它无法应用于同一个类超过一次。 `AttributeUsage` 是 <xref:System.AttributeUsageAttribute> 的别名。

有关详细信息，请参阅[使用反射访问特性 (Visual Basic)](accessing-attributes-by-using-reflection.md)。

## <a name="example"></a>示例

以下示例演示 `Inherited` 和 `AllowMultiple` 参数对 `AttributeUsage` 特性的影响，以及如何枚举应用于类的自定义特性。

```vb
' Create some custom attributes:
<AttributeUsage(System.AttributeTargets.Class, Inherited:=False)>
Class A1
    Inherits System.Attribute
End Class

<AttributeUsage(System.AttributeTargets.Class)>
Class A2
    Inherits System.Attribute
End Class

<AttributeUsage(System.AttributeTargets.Class, AllowMultiple:=True)>
Class A3
    Inherits System.Attribute
End Class

' Apply custom attributes to classes:
<A1(), A2()>
Class BaseClass

End Class

<A3(), A3()>
Class DerivedClass
    Inherits BaseClass
End Class

Public Class TestAttributeUsage
    Sub Main()
        Dim b As New BaseClass
        Dim d As New DerivedClass
        ' Display custom attributes for each class.
        Console.WriteLine("Attributes on Base Class:")
        Dim attrs() As Object = b.GetType().GetCustomAttributes(True)

        For Each attr In attrs
            Console.WriteLine(attr)
        Next

        Console.WriteLine("Attributes on Derived Class:")
        attrs = d.GetType().GetCustomAttributes(True)
        For Each attr In attrs
            Console.WriteLine(attr)
        Next
    End Sub
End Class
```

## <a name="sample-output"></a>示例输出

```console
Attributes on Base Class:
A1
A2
Attributes on Derived Class:
A3
A3
A2
```

## <a name="see-also"></a>另请参阅

- <xref:System.Attribute>
- <xref:System.Reflection>
- [Visual Basic 编程指南](../../index.md)
- [特性](../../../../standard/attributes/index.md)
- [反射 (Visual Basic)](../reflection.md)
- [特性 (Visual Basic)](../../../language-reference/attributes.md)
- [创建自定义特性 (Visual Basic)](creating-custom-attributes.md)
- [使用反射访问特性 (Visual Basic)](accessing-attributes-by-using-reflection.md)
