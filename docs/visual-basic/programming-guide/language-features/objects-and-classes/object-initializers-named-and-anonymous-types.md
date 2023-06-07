---
title: 对象初始值设定项：命名和匿名类型
ms.date: 07/20/2015
f1_keywords:
- vb.ObjectInitializer
helpviewer_keywords:
- object initializers [Visual Basic]
- anonymous types [Visual Basic], object initializers
- initializing properties [Visual Basic]
- initializers [Visual Basic]
- named types [Visual Basic]
ms.assetid: e2df3807-a70f-49dd-ac94-f1e07f472b1b
ms.openlocfilehash: 724407fed5bf90ed6e3e470cbabc9e42856cb99a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087474"
---
# <a name="object-initializers-named-and-anonymous-types-visual-basic"></a>对象初始值设定项：命名类型和匿名类型 (Visual Basic)

使用对象初始值设定项，您可以通过使用单个表达式来指定复杂对象的属性。 它们可用于创建命名类型和匿名类型的实例。  
  
## <a name="declarations"></a>声明  

 命名类型和匿名类型的实例的声明可能看起来几乎完全相同，但其效果并不相同。 每个类别都具有自己的功能和限制。 下面的示例演示如何 `Customer` 使用对象初始值设定项列表来声明和初始化命名类的实例。 请注意，类的名称在关键字之后指定 `New` 。  
  
 [!code-vb[VbVbalrObjectInit#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#1)]  
  
 匿名类型没有可用的名称。 因此，匿名类型的实例化不能包含类名。  
  
 [!code-vb[VbVbalrObjectInit#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#2)]  
  
 这两个声明的要求和结果不同。 对于 `namedCust` ， `Customer` 具有属性的类 `Name` 必须已经存在，并且声明将创建该类的实例。 对于 `anonymousCust` ，编译器会定义一个新类，该类具有一个属性和一个名为的字符串， `Name` 并创建该类的一个新实例。  
  
## <a name="named-types"></a>命名类型  

 对象初始值设定项提供一种简单的方法来调用类型的构造函数，然后在单个语句中设置部分或全部属性的值。 编译器将调用语句的相应构造函数：如果未提供任何参数，则为无参数构造函数; 或者，如果发送一个或多个参数，则为参数化构造函数。 之后，指定的属性将按照它们在初始值设定项列表中的显示顺序进行初始化。  
  
 初始值设定项列表中的每个初始化都包含将初始值分配给类的成员的。 成员的名称和数据类型是在定义类时确定的。 在下面的示例中， `Customer` 类必须存在，并且必须具有名为 `Name` 和 `City` 的成员，这些成员可以接受字符串值。  
  
 [!code-vb[VbVbalrObjectInit#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#3)]  
  
 或者，可以使用以下代码获取相同的结果：  
  
 [!code-vb[VbVbalrObjectInit#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#4)]  
  
 其中每个声明都等效于下面的示例，该示例 `Customer` 使用无参数构造函数创建对象，然后 `Name` 使用语句指定和属性的初始值 `City` `With` 。  
  
 [!code-vb[VbVbalrObjectInit#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#5)]  
  
 如果 `Customer` 类包含一个参数化构造函数，该构造函数使你能够在的值中发送 `Name` ，则你还可以 `Customer` 通过以下方式声明和初始化对象：  
  
 [!code-vb[VbVbalrObjectInit#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#6)]  
  
 您不必初始化所有属性，如以下代码所示。  
  
 [!code-vb[VbVbalrObjectInit#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#7)]  
  
 但是，初始化列表不能为空。 未初始化属性保留其默认值。  
  
### <a name="type-inference-with-named-types"></a>具有命名类型的类型推理  

 您可以 `cust1` 通过组合对象初始值设定项和局部类型推理，缩短的声明的代码。 这使您可以省略 `As` 变量声明中的子句。 变量的数据类型是从由赋值创建的对象的类型推断出来的。 在下面的示例中，的类型 `cust6` 为 `Customer` 。  
  
 [!code-vb[VbVbalrObjectInit#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#8)]  
  
### <a name="remarks-about-named-types"></a>有关命名类型的备注  
  
- 类成员不能在对象初始值设定项列表中多次初始化。 的声明 `cust7` 会导致错误。  
  
     [!code-vb[VbVbalrObjectInit#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#9)]  
  
- 成员可用于初始化自身或另一个字段。 如果在初始化之前访问成员，则 `cust8` 将使用默认值，如在的以下声明中一样。 请记住，当处理使用对象初始值设定项的声明时，首先要执行的操作是调用适当的构造函数。 然后，初始化初始值设定项列表中的各个字段。 在下面的示例中，为分配了的默认值 `Name` `cust8` ，并在中分配了一个初始化值 `cust9` 。  
  
     [!code-vb[VbVbalrObjectInit#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#10)]  
  
     下面的示例使用和中的参数化构造函数 `cust3` `cust4` 来声明和初始化 `cust10` 和 `cust11` 。  
  
     [!code-vb[VbVbalrObjectInit#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#11)]  
  
- 对象初始值设定项可以嵌套。 在下面的示例中， `AddressClass` 是一个具有两个属性（ `City` 和）的类， `State` 并且 `Customer` 类具有一个作为 `Address` 实例的属性 `AddressClass` 。  
  
     [!code-vb[VbVbalrObjectInit#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#12)]  
  
- 初始化列表不能为空。  
  
- 正在初始化的实例不能是 Object 类型。  
  
- 要初始化的类成员不能为共享成员、只读成员、常量或方法调用。  
  
- 无法对初始化的类成员进行索引或限定。 下面的示例引发编译器错误：  
  
     `'' Not valid.`  
  
     `' Dim c1 = New Customer With {.OrderNumbers(0) = 148662}`  
  
     `' Dim c2 = New Customer with {.Address.City = "Springfield"}`  
  
## <a name="anonymous-types"></a>匿名类型  

 匿名类型使用对象初始值设定项来创建未显式定义和命名的新类型的实例。 相反，编译器将根据您在对象初始值设定项列表中指定的属性生成类型。 由于未指定类型的名称，因此称为 *匿名类型*。 例如，将以下声明与的早期声明进行比较 `cust6` 。  
  
 [!code-vb[VbVbalrObjectInit#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#13)]  
  
 唯一的不同之处在于，不在数据类型后指定名称 `New` 。 但发生了什么变化。 编译器会定义一个新的匿名类型，该类型具有两个属性： `Name` 和 `City` ，并使用指定的值创建它的实例。 类型推理确定 `Name` `City` 要作为字符串的示例中和的类型。  
  
> [!CAUTION]
> 匿名类型的名称由编译器生成，并且可能因编译而异。 你的代码不应使用或依赖于匿名类型的名称。  
  
 由于类型名称不可用，因此不能使用 `As` 子句来声明 `cust13` 。 必须推断其类型。 如果不使用后期绑定，则这会将匿名类型限制为本地变量。  
  
 匿名类型为 LINQ 查询提供关键支持。 有关在查询中使用匿名类型的详细信息，请参阅 Visual Basic 中的 [匿名类型](anonymous-types.md) 和 [LINQ 简介](../linq/introduction-to-linq.md)。  
  
### <a name="remarks-about-anonymous-types"></a>有关匿名类型的备注  
  
- 通常，匿名类型声明中的所有属性或大多数属性都是关键属性，这些属性通过 `Key` 在属性名称前面键入关键字来指示。  
  
     [!code-vb[VbVbalrObjectInit#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#14)]  
  
     有关密钥属性的详细信息，请参阅 " [密钥](../../../language-reference/modifiers/key.md)"。  
  
- 与命名类型一样，匿名类型定义的初始化表达式列表必须声明至少一个属性。  
  
     [!code-vb[VbVbalrObjectInit#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#2)]  
  
- 声明匿名类型的实例时，编译器将生成匹配的匿名类型定义。 属性的名称和数据类型取自实例声明，由编译器包含在定义中。 属性并未事先命名和定义，因为它们适用于命名类型。 它们的类型会被推断。 不能使用子句来指定属性的数据类型 `As` 。  
  
- 匿名类型还可以通过多种其他方式建立其属性的名称和值。 例如，匿名类型属性可以同时采用变量的名称和值，也可以同时采用另一对象的属性的名称和值。  
  
     [!code-vb[VbVbalrObjectInit#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#15)]  
  
     有关在匿名类型中定义属性的选项的详细信息，请参阅 [如何：推断匿名类型声明中的属性名称和类型](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)。  
  
## <a name="see-also"></a>请参阅

- [局部类型推理](../variables/local-type-inference.md)
- [匿名类型](anonymous-types.md)
- [Visual Basic 中的 LINQ 简介](../linq/introduction-to-linq.md)
- [如何：推断匿名类型声明中的属性名和类型](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [Key](../../../language-reference/modifiers/key.md)
- [如何：使用对象初始值设定项声明对象](how-to-declare-an-object-by-using-an-object-initializer.md)
