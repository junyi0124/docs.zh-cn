---
title: Class 语句
ms.date: 05/12/2018
f1_keywords:
- vb.Class
helpviewer_keywords:
- class modules
- Class statement [Visual Basic]
- classes [Visual Basic], fields
- fields [Visual Basic], of classes
- class types [Visual Basic], class statements
- classes [Visual Basic], creating
- classes [Visual Basic], data members
- data members [Visual Basic], of classes
ms.assetid: f2664f38-eb5a-4d4b-a374-1d041521fb6c
ms.openlocfilehash: 3b64597fcd7453c20ed295fe263eeaa8783b20ae
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866038"
---
# <a name="class-statement-visual-basic"></a>Class 语句 (Visual Basic)

声明类的名称，并引入类所包含的变量、属性、事件和过程的定义。  
  
## <a name="syntax"></a>语法  
  
```vb  
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ MustInherit | NotInheritable ] [ Partial ] _  
Class name [ ( Of typelist ) ]  
    [ Inherits classname ]  
    [ Implements interfacenames ]  
    [ statements ]  
End Class  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`attributelist`|可选。 请参阅 [特性列表](attribute-list.md)。|  
|`accessmodifier`|可选。 可以是以下值之一：<br /><br /> -   [公布](../modifiers/public.md)<br />-   [避免](../modifiers/protected.md)<br />-   [友好](../modifiers/friend.md)<br />-   [专有](../modifiers/private.md)<br />-   [受保护的朋友](../modifiers/protected-friend.md)<br />- [私有受保护](../modifiers/private-protected.md)<br/><br/> 请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。|  
|`Shadows`|可选。 请参阅 [阴影](../modifiers/shadows.md)。|  
|`MustInherit`|可选。 请参阅 [MustInherit](../modifiers/mustinherit.md)。|  
|`NotInheritable`|可选。 请参阅 [NotInheritable](../modifiers/notinheritable.md)。|  
|`Partial`|可选。 指示类的分部定义。 请参阅 [部分](../modifiers/partial.md)。|  
|`name`|必需。 此类的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。|  
|`Of`|可选。 指定这是一个泛型类。|  
|`typelist`|如果使用 of 关键字，则 [为](of-clause.md) 必需。 此类的类型参数列表。 请参阅 [类型列表](type-list.md)。|  
|`Inherits`|可选。 指示此类继承另一个类的成员。 请参阅 [Inherits 语句](inherits-statement.md)。|  
|`classname`|如果使用语句，则为必需 `Inherits` 。 此类派生自的类的名称。|  
|`Implements`|可选。 指示此类实现一个或多个接口的成员。 请参阅 [Implements 语句](implements-statement.md)。|  
|`interfacenames`|如果使用语句，则为必需 `Implements` 。 此类实现的接口的名称。|  
|`statements`|可选。 定义此类的成员的语句。|  
|`End Class`|必需。 终止 `Class` 定义。|  
  
## <a name="remarks"></a>备注  

 `Class`语句定义新的数据类型。 *类*是面向对象编程的基本构建基块 (OOP) 。 有关详细信息，请参阅 [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)。  
  
 只能 `Class` 在命名空间或模块级别使用。 这意味着类的 *声明上下文* 必须是源文件、命名空间、类、结构、模块或接口，不能是过程或块。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。  
  
 类的每个实例都具有独立于所有其他实例的生存期。 此生存期在由 [新运算符](../operators/new-operator.md) 子句或函数（如）创建时开始 <xref:Microsoft.VisualBasic.Interaction.CreateObject%2A> 。 当指向实例的所有变量均设置为 [Nothing](../nothing.md) 或其他类的实例时，它将结束。  
  
 类默认为 [Friend](../modifiers/friend.md) 访问。 您可以使用访问修饰符调整其访问级别。 有关详细信息，请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。  
  
## <a name="rules"></a>规则  
  
- **层.** 可以在另一个类中定义一个类。 外部类称为 *包含类*，内部类称为 " *嵌套类*"。  
  
- **继承.** 如果类使用 [Inherits 语句](inherits-statement.md)，则只能指定一个基类或接口。 类不能从多个元素继承。  
  
     类不能继承自具有限制性更强的访问级别的另一个类。 例如， `Public` 类不能从 `Friend` 类继承。  
  
     类不能从嵌套在它中的类继承。  
  
- **部署.** 如果类使用 [Implements 语句](implements-statement.md)，则必须实现在中指定的每个接口所定义的每个成员 `interfacenames` 。 这种情况的一个例外是基类成员的重新实现。 有关详细信息，请参阅 [实现](implements-clause.md)中的 "重新实现"。  
  
- **默认属性。** 一个类最多只能指定一个属性作为其 *默认属性*。 有关详细信息，请参阅 [默认值](../modifiers/default.md)。  
  
## <a name="behavior"></a>行为  
  
- **访问级别。** 在类中，可以使用其自己的访问级别声明每个成员。 类成员默认为 [公共](../modifiers/public.md) 访问，变量和常量除外，默认为 [私有](../modifiers/private.md) 访问。 当某个类的访问权限超过其成员之一时，类访问级别将优先。  
  
- **内.** 类在其包含的命名空间、类、结构或模块的范围内。  
  
     每个类成员的作用域都是整个类。  
  
     **生存期.** Visual Basic 不支持静态类。 与静态类等效的功能由模块提供。 有关详细信息，请参阅 [Module 语句](module-statement.md)。  
  
     类成员的生存期取决于其声明的方式和位置。 有关详细信息，请参阅 [Visual Basic 中的生存期](../../programming-guide/language-features/declared-elements/lifetime.md)。  
  
- **限定.** 类之外的代码必须使用该类的名称来限定成员的名称。  
  
     如果嵌套类中的代码对编程元素进行了非限定引用，则 Visual Basic 首先在嵌套类中搜索元素，然后在其包含类中搜索元素，并将其放入外部的包含元素。  
  
## <a name="classes-and-modules"></a>类和模块  

 这些元素具有许多相似之处，但也存在一些重要的差异。  
  
- **术语。** Visual Basic 的早期版本可识别两种类型的模块： *类模块* ( cls 文件) 和 *标准模块* ( bas 文件) 。 当前版本分别调用这些 *类* 和 *模块*。  
  
- **共享成员。** 您可以控制某个类的成员是共享成员还是实例成员。  
  
- **对象方向。** 类是面向对象的，但模块不是。 你可以创建一个或多个类的实例。 有关详细信息，请参阅 [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Class` 语句定义一个类和多个成员。  
  
 [!code-vb[VbVbalrStatements#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#62)]  
  
## <a name="see-also"></a>另请参阅

- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
- [结构和类](../../programming-guide/language-features/data-types/structures-and-classes.md)
- [Interface 语句](interface-statement.md)
- [Module 语句](module-statement.md)
- [Property Statement](property-statement.md)
- [对象生存期：如何创建和销毁对象](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [如何：使用泛型类](../../programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
