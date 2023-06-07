---
title: 如何：定义可对不同数据类型提供相同功能的类
ms.date: 07/20/2015
helpviewer_keywords:
- data type arguments [Visual Basic], using
- type parameters [Visual Basic], defining
- data type arguments [Visual Basic], defining
- arguments [Visual Basic], data types
- Of keyword [Visual Basic], using
- constraints, Visual Basic generic types
- generic parameters
- data type parameters
- data type parameters [Visual Basic], using
- generics [Visual Basic], defining classes with type parameters
- data types [Visual Basic], as parameters
- data types [Visual Basic], as arguments
- parameters [Visual Basic], type
- type arguments
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- type parameters
- data type arguments
- parameters [Visual Basic], data type
- generics [Visual Basic], defining generic types
- data type parameters [Visual Basic], defining
- type arguments [Visual Basic], defining
- arguments [Visual Basic], type
ms.assetid: a914adf8-e68f-4819-a6b1-200d1cf1c21c
ms.openlocfilehash: 268daf333dc5463e5436304cec188a9e6d477166
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077119"
---
# <a name="how-to-define-a-class-that-can-provide-identical-functionality-on-different-data-types-visual-basic"></a>如何：定义可对不同数据类型提供相同功能的类 (Visual Basic)

你可以定义这样一个类：你可以通过该类创建可在不同数据类型上提供相同功能的对象。 为此，你可以在定义中指定一个或多个 *类型形参* 。 然后，该类将能够充当使用不同数据类型的对象的模板。 通过这种方式定义的类称为 *泛型类*。  
  
 定义泛型类这种做法的优点在于：你只需定义一次泛型类，代码便可以利用它来创建使用各种数据类型的多个对象。 相对于使用 `Object` 类型定义类而言，这样做的性能将会更好。  
  
 除了类之外，你还可以定义和使用泛型结构、接口、过程和委托。  
  
### <a name="to-define-a-class-with-a-type-parameter"></a>使用类型形参定义类  
  
1. 采用常规方式定义类。  
  
2. `(Of` *typeparameter* `)` 紧靠类名称之后添加 typeparameter，以指定一个类型参数。  
  
3. 如果有一个以上的类型形参，请在括号内列出这些参数（以逗号分隔）。 不要重复 `Of` 关键字。  
  
4. 如果代码是对类型形参执行操作，而不是简单的赋值，请在该类型形参后添加一个 `As` 子句，以便添加一个或多个 *约束*。 约束可保证为该类型形参提供的类型满足如下所示的要求：  
  
    - 支持代码执行的运算（如 `>`）  
  
    - 支持代码访问的成员（如方法）  
  
    - 公开无参数构造函数  
  
     如果未指定任何约束，则代码只能使用 [Object Data Type](../../../language-reference/data-types/object-data-type.md)支持的那些运算和成员。 有关详细信息，请参阅 [Type List](../../../language-reference/statements/type-list.md)。  
  
5. 标识要使用所提供类型声明的每个类成员，然后将其声明为 `As` `typeparameter` 。 这适用于内部存储、过程参数和返回值。  
  
6. 确保代码只使用它可提供给 `itemType`的任何数据类型所支持的运算和方法。  
  
     下面的示例定义了一个类，用于管理一个非常简单的列表。 它将列表保存在内部数组 `items`中，并且使用代码可声明列表元素的数据类型。 参数化构造函数允许使用代码设置的上限 `items` ，而无参数构造函数将此项设置为9个 (，总计10个项) 。  
  
     [!code-vb[VbVbalrDataTypes#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#7)]  
  
     你可以声明来自 `simpleList` 的一个类来保存 `Integer` 值的列表，声明另一个类来保存 `String` 值的列表，再声明一个类来保存 `Date` 值。 除了列表成员的数据类型外，依据所有这些类创建的对象的行为方式都相同。  
  
     所用代码提供给 `itemType` 的类型实参可以是内部类型（如 `Boolean` 或 `Double`）、结构、枚举或任何类型的类，包括应用程序定义的类。  
  
     可以使用以下代码测试类 `simpleList` 。  
  
     [!code-vb[VbVbalrDataTypes#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#8)]  
  
## <a name="see-also"></a>请参阅

- [数据类型](index.md)
- [Generic Types in Visual Basic](generic-types.md)
- [语言独立性和与语言无关的组件](../../../../standard/language-independence-and-language-independent-components.md)
- [个](../../../language-reference/statements/of-clause.md)
- [Type List](../../../language-reference/statements/type-list.md)
- [如何：使用泛型类](how-to-use-a-generic-class.md)
- [Object Data Type](../../../language-reference/data-types/object-data-type.md)
