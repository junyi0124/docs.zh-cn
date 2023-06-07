---
title: 对象变量声明
ms.date: 07/20/2015
helpviewer_keywords:
- early binding [Visual Basic]
- declarations [Visual Basic], class
- classes [Visual Basic], declaring
- binding [Visual Basic], late and early
- object variables [Visual Basic], declaring
- variables [Visual Basic], object
- declaring variables [Visual Basic], object variables
- declaring classes [Visual Basic]
- late binding [Visual Basic]
ms.assetid: 2a5a41a3-1aa8-4236-b1f0-2382af7bf715
ms.openlocfilehash: 74b1401df3dbb2d744de74734d10cbcd92e9689e
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077041"
---
# <a name="object-variable-declaration-visual-basic"></a>对象变量声明 (Visual Basic)

使用 normal 声明语句声明对象变量。 对于数据类型，指定 `Object` (，即 [对象数据类型](../../../language-reference/data-types/object-data-type.md)) 或要从中创建对象的更具体的类。  
  
 将变量声明为与 `Object` 将其声明为相同 <xref:System.Object?displayProperty=nameWithType> 。  
  
 当使用特定的对象类声明变量时，它可以访问由此类及其继承的类公开的所有方法和属性。 如果使用声明变量 <xref:System.Object> ，则它只能访问类的成员 <xref:System.Object> ，除非你启用 `Option Strict Off` 后期绑定。  
  
## <a name="declaration-syntax"></a>声明语法  

 使用以下语法声明对象变量：  
  
```vb  
Dim variablename As [New] { objectclass | Object }  
```  
  
 你还可以在声明中指定 [Public](../../../language-reference/modifiers/public.md)、 [Protected](../../../language-reference/modifiers/protected.md)、 [Friend](../../../language-reference/modifiers/friend.md)、 `Protected Friend` 、 [Private](../../../language-reference/modifiers/private.md)、 [Shared](../../../language-reference/modifiers/shared.md)或 [Static](../../../language-reference/modifiers/static.md) 。 下面的示例声明是有效的：  
  
```vb  
Private objA As Object  
Static objB As System.Windows.Forms.Label  
Dim objC As System.OperatingSystem  
```  
  
## <a name="late-binding-and-early-binding"></a>后期绑定和早期绑定  

 有时，在代码运行之前，特定类是未知的。 在这种情况下，必须声明数据类型的对象变量 `Object` 。 这会创建对任何类型的对象的常规引用，并在运行时分配特定类。 这称为 *后期绑定*。 后期绑定需要额外的执行时间。 它还将代码限制为最近分配给它的类的方法和属性。 如果你的代码尝试访问不同类的成员，这可能会导致运行时错误。  
  
 如果在编译时知道特定类，则应将对象变量声明为该类的。 这称为*早期绑定*。 早期绑定可提高性能，并保证代码访问特定类的所有方法和属性。 在前面的示例声明中，如果变量 `objA` 仅使用类的对象 <xref:System.Windows.Forms.Label?displayProperty=nameWithType> ，则应 `As System.Windows.Forms.Label` 在其声明中指定。  
  
### <a name="advantages-of-early-binding"></a>早期绑定的优点  

 将对象变量声明为特定类具有以下优势：  
  
- 自动类型检查  
  
- 保证能够访问特定类的所有成员  
  
- 代码编辑器中的 Microsoft IntelliSense 支持  
  
- 提高代码的可读性  
  
- 代码中的错误越少  
  
- 在编译时（而非运行时）捕获的错误  
  
- 更快的代码执行  
  
## <a name="access-to-object-variable-members"></a>对对象变量成员的访问  

 当 `Option Strict` 启用时 `On` ，对象变量仅可访问声明它所用的类的方法和属性。 下面的示例对此进行了演示。  
  
```vb  
' Option statements must precede all other source file lines.  
Option Strict On  
' Imports statement must precede all declarations in the source file.  
Imports System.Windows.Forms  
Public Sub accessMembers()  
    Dim p As Object  
    Dim q As System.Windows.Forms.Label  
    p = New System.Windows.Forms.Label  
    q = New System.Windows.Forms.Label  
    Dim j, k As Integer  
    ' The following statement generates a compiler ERROR.  
    j = p.Left  
    ' The following statement retrieves the left edge of the label in pixels.  
    k = q.Left  
End Sub  
```  
  
 在此示例中， `p` 仅可使用 <xref:System.Object> 类的成员，这不包括 `Left` 属性。 另一方面， `q` 已声明为 <xref:System.Windows.Forms.Label>类型，因此它可以使用 <xref:System.Windows.Forms.Label> 命名空间中 <xref:System.Windows.Forms> 类的所有方法和属性。  
  
## <a name="flexibility-of-object-variables"></a>对象变量的灵活性  

 使用继承层次结构中的对象时，可以选择使用哪个类来声明对象变量。 在做出此选择的过程中，必须在对象赋值的灵活性与对类成员的访问之间取得平衡。 例如，请考虑导致类的继承层次结构 <xref:System.Windows.Forms.Form?displayProperty=nameWithType> ：  
  
 <xref:System.Object>  
  
 &nbsp;&nbsp;<xref:System.MarshalByRefObject>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;<xref:System.ComponentModel.Component>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.Control>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.ScrollableControl>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.ContainerControl>  
  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.Windows.Forms.Form>  
  
 假设你的应用程序定义了一个名为的窗体类 `specialForm` ，该类继承自类 <xref:System.Windows.Forms.Form> 。 您可以声明一个专门引用的对象变量 `specialForm` ，如下面的示例所示。  
  
```vb  
Public Class specialForm  
    Inherits System.Windows.Forms.Form  
    ' Insert code defining methods and properties of specialForm.  
End Class  
Dim nextForm As New specialForm  
```  
  
 前面的示例中的声明将变量限制 `nextForm` 为类的对象 `specialForm` ，但它还使可用于的所有方法和属性，以及继承的所有类的所有 `specialForm` `nextForm` 成员 `specialForm` 。  
  
 可以通过将对象变量声明为类型来使对象变量更通用 <xref:System.Windows.Forms.Form> ，如下面的示例所示。  
  
```vb  
Dim anyForm As System.Windows.Forms.Form  
```  
  
 前面的示例中的声明使你可以将应用程序中的任何表单分配给 `anyForm` 。 但是，虽然 `anyForm` 可以访问类的所有成员 <xref:System.Windows.Forms.Form> ，但它不能使用为特定窗体定义的任何其他方法或属性（如） `specialForm` 。  
  
 基类的所有成员均可用于派生类，但派生类的其他成员对基类不可用。  
  
## <a name="see-also"></a>请参阅

- [对象变量](object-variables.md)
- [对象变量赋值](object-variable-assignment.md)
- [对象变量值](object-variable-values.md)
- [如何：在 Visual Basic 中声明对象变量并为它分配对象](how-to-declare-an-object-variable-and-assign-an-object-to-it.md)
- [如何：访问对象的成员](how-to-access-members-of-an-object.md)
- [New 运算符](../../../language-reference/operators/new-operator.md)
- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
- [局部类型推理](local-type-inference.md)
