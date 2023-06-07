---
title: MustInherit
ms.date: 07/20/2015
f1_keywords:
- MustInherit
- vb.MustInherit
helpviewer_keywords:
- classes [Visual Basic], abstract
- MustInherit classes [Visual Basic], MustInherit keyword
- abstract classes [Visual Basic], MustInherit class
- MustInherit keyword [Visual Basic]
ms.assetid: b8f05185-90e3-4dd7-adc2-90d852fab5b4
ms.openlocfilehash: 6502da947ae331a26e66d8ce2dbcda46e4172a6e
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867956"
---
# <a name="mustinherit-visual-basic"></a>MustInherit (Visual Basic)

指定类只能用作基类，并且不能直接从其创建对象。  
  
## <a name="remarks"></a>备注  

 *基类*的目的 (也称为*抽象类*) 是定义派生自它的所有类所共有的功能。 这会保存派生类，使其不必重新定义公共元素。 在某些情况下，这种通用功能不够完整，无法生成可用的对象，并且每个派生类都定义了缺少的功能。 在这种情况下，你希望使用代码仅在派生类中创建对象。 在 `MustInherit` 基类上使用可以强制执行此类。  
  
 类的另一种用法 `MustInherit` 是将变量限制为一组相关的类。 你可以定义一个基类，并从其派生所有这些相关的类。 基类不需要提供所有派生类共有的任何功能，但它可以充当用于向变量赋值的筛选器。 如果你使用的代码将变量声明为基类，则 Visual Basic 允许你仅将一个派生类中的对象分配给该变量。  
  
 .NET Framework 定义了多个类，其中包括、 `MustInherit` <xref:System.Array> <xref:System.Enum> 和 <xref:System.ValueType> 。 <xref:System.ValueType> 是限制变量的基类的一个示例。 所有值类型都是从派生的 <xref:System.ValueType> 。 如果将变量声明为 <xref:System.ValueType> ，则只能将值类型分配给该变量。  
  
## <a name="rules"></a>规则  
  
- **声明上下文。** `MustInherit`只能在语句中使用 `Class` 。  
  
- **组合修饰符。** 不能 `MustInherit` `NotInheritable` 在同一声明中同时指定和。  
  
## <a name="example"></a>示例  

 下面的示例演示强制的继承和强制重写。 基类 `shape` 定义变量 `acrossLine` 。 类 `circle` 和 `square` 派生自 `shape` 。 它们继承的定义 `acrossLine` ，但必须定义函数， `area` 因为每种形状的计算都是不同的。  
  
 [!code-vb[VbVbalrKeywords#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#2)]  
  
 可以 `shape1` 将和声明 `shape2` 为类型 `shape` 。 但是，不能从创建对象， `shape` 因为它缺少函数的功能 `area` ，并被标记为 `MustInherit` 。  
  
 由于它们被声明为 `shape` ，因此，变量 `shape1` 和 `shape2` 仅限于派生类和中的 `circle` 对象 `square` 。 Visual Basic 不允许将任何其他对象分配给这些变量，这为你提供了高级别的类型安全性。  
  
## <a name="usage"></a>使用情况  

 `MustInherit`可以在此上下文中使用修饰符：  
  
 [Class 语句](../statements/class-statement.md)  
  
## <a name="see-also"></a>另请参阅

- [Inherits Statement](../statements/inherits-statement.md)
- [NotInheritable](notinheritable.md)
- [关键字](../keywords/index.md)
- [继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
