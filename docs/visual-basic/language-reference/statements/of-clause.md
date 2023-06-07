---
title: Of 子句
ms.date: 07/20/2015
f1_keywords:
- Of
- vb.Of
- vb.of
helpviewer_keywords:
- Of keyword [Visual Basic]
- arguments [Visual Basic], data types
- constraints, Visual Basic generic types
- generic parameters
- generics [Visual Basic], constraints
- parameters [Visual Basic], type
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- type parameters
- data type arguments
ms.assetid: 0db8f65c-65af-4089-ab7f-6fcfecb60444
ms.openlocfilehash: 0595356fb75fc0ac73a49622d71fe1d28fa7b648
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90865902"
---
# <a name="of-clause-visual-basic"></a>Of 子句 (Visual Basic)

引入一个 `Of` 子句，该子句标识*泛型*类、结构、接口、委托或过程中的*类型参数*。 有关泛型类型的信息，请参阅 [Visual Basic 中的泛型类型](../../programming-guide/language-features/data-types/generic-types.md)。  
  
## <a name="using-the-of-keyword"></a>使用关键字 of  

 下面的代码示例使用 `Of` 关键字定义带有两个类型参数的类的轮廓。 它*constrains* `keyType` 通过接口约束参数 <xref:System.IComparable> ，这意味着使用的代码必须提供实现的类型参数 <xref:System.IComparable> 。 这是必需的，以便 `add` 过程可以调用 <xref:System.IComparable.CompareTo%2A?displayProperty=nameWithType> 方法。 有关约束的详细信息，请参阅 [Type List](type-list.md)。  
  
```vb  
Public Class Dictionary(Of entryType, keyType As IComparable)  
    Public Sub add(ByVal e As entryType, ByVal k As keyType)  
        Dim dk As keyType  
        If k.CompareTo(dk) = 0 Then  
        End If  
    End Sub  
    Public Function find(ByVal k As keyType) As entryType  
    End Function  
End Class  
```  
  
 如果你完成了前面的类定义，则可以构造其中的各种 `dictionary` 类。 提供给的类型， `entryType` 并 `keyType` 确定类包含的项的类型以及它与每个项关联的键的类型。 由于约束的原因，您必须提供 `keyType` 实现的类型 <xref:System.IComparable> 。  
  
 下面的代码示例创建一个对象，该对象保存 `String` 项并将 `Integer` 键与每个项关联。 `Integer` 实现 <xref:System.IComparable> 并因此满足上的约束 `keyType` 。  
  
```vb  
Dim d As New dictionary(Of String, Integer)  
```  
  
 `Of` 关键字可用于以下上下文中：  
  
 [Class 语句](class-statement.md)  
  
 [Delegate 语句](delegate-statement.md)  
  
 [Function 语句](function-statement.md)  
  
 [Interface 语句](interface-statement.md)  
  
 [Structure 语句](structure-statement.md)  
  
 [Sub 语句](sub-statement.md)  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IComparable>
- [Type List](type-list.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [位于](../modifiers/in-generic-modifier.md)
- [弄](../modifiers/out-generic-modifier.md)
