---
title: 部分
ms.date: 07/20/2015
f1_keywords:
- vb.Partial
- partial
helpviewer_keywords:
- structures [Visual Basic], partial
- classes [Visual Basic]
- partial, types [Visual Basic]
- partial, structures
- partial, classes [Visual Basic]
- classes [Visual Basic], partial
- Partial keyword [Visual Basic]
- type promotion
ms.assetid: 7adaef80-f435-46e1-970a-269fff63b448
ms.openlocfilehash: 2482facadd0e0528ed1b71df6edb4a447947a902
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867782"
---
# <a name="partial-visual-basic"></a>分部 (Visual Basic)

指示类型声明为类型的分部定义。  
  
 可以使用 `Partial` 关键字在几个声明之间划分类型的定义。 可以根据需要在任意数量的不同源文件中使用任意数量的分部声明。 但是，所有声明都必须在相同的程序集和相同的命名空间中。  
  
> [!NOTE]
> Visual Basic 支持 *分部方法*，这些方法通常在分部类中实现。 有关详细信息，请参阅 [分部方法](../../programming-guide/language-features/procedures/partial-methods.md) 和 [Sub 语句](../statements/sub-statement.md)。  
  
## <a name="syntax"></a>语法  
  
```vb  
[ <attrlist> ] [ accessmodifier ] [ Shadows ] [ MustInherit | NotInheritable ] _  
Partial { Class | Structure | Interface | Module } name [ (Of typelist) ]  
    [ Inherits classname ]  
    [ Implements interfacenames ]  
    [ variabledeclarations ]  
    [ proceduredeclarations ]  
{ End Class | End Structure }  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`attrlist`|可选。 应用于此类型的特性列表。 必须将 [属性列表](../statements/attribute-list.md) 用尖括号括起来 (`< >`) 。|  
|`accessmodifier`|可选。 指定哪些代码可以访问此类型。 请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。|  
|`Shadows`|可选。 请参阅 [阴影](shadows.md)。|  
|`MustInherit`|可选。 请参阅 [MustInherit](mustinherit.md)。|  
|`NotInheritable`|可选。 请参阅 [NotInheritable](notinheritable.md)。|  
|`name`|必需。 此类型的名称。 必须匹配在同一类型的所有其他分部声明中定义的名称。|  
|`Of`|可选。 指定这是一种泛型类型。 请参阅 [Visual Basic 中的泛型类型](../../programming-guide/language-features/data-types/generic-types.md)。|  
|`typelist`|如果使用，则 [是必需的](../statements/of-clause.md)。 请参阅 [类型列表](../statements/type-list.md)。|  
|`Inherits`|可选。 请参阅 [Inherits 语句](../statements/inherits-statement.md)。|  
|`classname`|如果使用 `Inherits`，则是必需的。 派生此类的类或接口的名称。|  
|`Implements`|可选。 请参阅 [Implements 语句](../statements/implements-statement.md)。|  
|`interfacenames`|如果使用 `Implements`，则是必需的。 此类型实现的接口的名称。|  
|`variabledeclarations`|可选。 声明该类型的其他变量和事件的语句。|  
|`proceduredeclarations`|可选。 声明和定义该类型的其他过程的语句。|  
|`End Class` 或 `End Structure`|结束此分部 `Class` 或 `Structure` 定义。|  
  
## <a name="remarks"></a>备注  

 Visual Basic 使用分部类定义将生成的代码与单独的源文件中用户编写的代码分开。 例如，“Windows 窗体设计器”**** 定义了控件的分部类，如 <xref:System.Windows.Forms.Form>。 不应修改这些控件中生成的代码。  
  
 创建分部类型时，将应用类、结构、接口和模块创建的所有规则（如用于修饰符的使用和继承的规则）。  
  
## <a name="best-practices"></a>最佳实践  
  
- 正常情况下，不应将单个类型的开发分跨两个或多个声明。 因此，在大多数情况下不需要 `Partial` 关键字。  
  
- 为了提高可读性，类型的每个分部声明都应包含 `Partial` 关键字。 编译器最多允许一个分部声明省略关键字；如果两个或多个分部声明省略了关键字，则编译器将发出错误信号。  
  
## <a name="behavior"></a>行为  
  
- **声明的联合。** 编译器将类型视为其所有分部声明的联合。 每个分部定义的每个修饰符均可应用于整个类型，并且每个分部定义的每个成员均可用于整个类型。  
  
- **模块中不允许使用分部类型的类型提升。** 如果分部定义位于模块内，则该类型的类型提升会自动失效。 在这种情况下，一组分部定义可能导致意外的结果，甚至导致编译器错误。 有关详细信息，请参阅 [类型提升](../../programming-guide/language-features/declared-elements/type-promotion.md)。  
  
     仅当其完全限定的路径相同时，编译器才将合并分部定义。  
  
 `Partial` 关键字可用于以下上下文中：  
  
 [Class 语句](../statements/class-statement.md)  
  
 [Structure 语句](../statements/structure-statement.md)  
  
## <a name="example"></a>示例  

 下面的示例将类 `sampleClass` 的定义拆分到两个声明中，其中每一个均定义一个不同的 `Sub` 过程。  
  
 [!code-vb[VbVbalrKeywords#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#3)]  
  
 前面示例中的两个分部定义可能在同一源文件中或在两个不同的源文件中。  
  
## <a name="see-also"></a>另请参阅

- [Class 语句](../statements/class-statement.md)
- [Structure 语句](../statements/structure-statement.md)
- [类型提升](../../programming-guide/language-features/declared-elements/type-promotion.md)
- [Shadows](shadows.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [分部方法](../../programming-guide/language-features/procedures/partial-methods.md)
