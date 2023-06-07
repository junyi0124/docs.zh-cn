---
title: 结构和类
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], vs. structures
- structures [Visual Basic]
- classes [Visual Basic]
- structures [Visual Basic], compared to classes
- structures [Visual Basic], structure variables
- structure variables [Visual Basic]
ms.assetid: a221e74a-ffcf-4bdc-a0f6-a088a9bf26cc
ms.openlocfilehash: e7ca5b9d55611eafad88517e71f9807fe2aa4416
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086213"
---
# <a name="structures-and-classes-visual-basic"></a>结构和类 (Visual Basic)

Visual Basic 统一了结构和类的语法，因此，这两个实体支持大多数相同的功能。 但结构和类之间也存在重要差异。  
  
 类的优点是引用类型，传递引用比将结构变量传递到其所有数据更有效。 另一方面，结构不需要在全局堆上分配内存。  
  
 由于不能从结构继承，因此结构应仅用于不需要扩展的对象。 如果要创建的对象的实例大小较小，请使用结构，并考虑类与结构的性能特征。  
  
## <a name="similarities"></a>相似性  

 结构和类在以下方面类似：  
  
- 两者都是 *容器* 类型，这意味着它们包含其他类型作为成员。  
  
- 两者都具有成员，其中可以包含构造函数、方法、属性、字段、常量、枚举、事件和事件处理程序。 但是，不要将这些成员与结构的已声明 *元素* 混淆。  
  
- 两者的成员可以具有不同的访问级别。 例如，一个成员可以声明， `Public` 另一个成员 `Private` 。  
  
- 两者均可实现接口。  
  
- 两者都可以具有共享构造函数（带有或不带参数）。  
  
- 两者都可以公开 *默认属性*，前提是该属性至少采用一个参数。  
  
- 两者都可以声明和引发事件，并且两者都可以声明委托。  
  
## <a name="differences"></a>差异  

 结构和类在以下方面有所不同：  
  
- 结构是 *值类型*;类是 *引用类型*。 结构类型的变量包含结构的数据，而不是包含对作为类类型的数据的引用。  
  
- 结构使用堆栈分配;类使用堆分配。  
  
- 默认情况下，所有结构元素都是默认的; 默认情况下 `Public` ，类变量和常量是 `Private` 其他类成员 `Public` 。 类成员的这种行为提供与 Visual Basic 6.0 系统的默认值的兼容性。  
  
- 结构中必须至少有一个非共享变量或非共享的非自定义事件元素;类可以完全为空。  
  
- 不能将结构元素声明为 `Protected` ; 类成员可以。  
  
- 仅当结构过程是[共享](../../../language-reference/modifiers/shared.md) `Sub` 过程，并且仅通过[AddHandler 语句](../../../language-reference/statements/addhandler-statement.md)的方式处理时，结构过程才能处理事件; 任何类过程都可以使用[Handles](../../../language-reference/statements/handles-clause.md)关键字或语句处理事件 `AddHandler` 。 有关详细信息，请参阅[事件](../events/index.md)。  
  
- 结构变量声明不能为数组指定初始值设定项或初始大小;类变量声明可以。  
  
- 结构隐式继承自 <xref:System.ValueType?displayProperty=nameWithType> 类，且不能从任何其他类型继承; 类可以继承自以外的任何类 <xref:System.ValueType?displayProperty=nameWithType> 。  
  
- 结构不可继承;类为。  
  
- 结构永远都不会终止，因此，公共语言运行时 (CLR) 从不对 <xref:System.Object.Finalize%2A> 任何结构调用方法; 类由垃圾回收器 (GC) 终止，后者 <xref:System.Object.Finalize%2A> 在检测到没有剩余的活动引用时调用类。  
  
- 结构不需要构造函数;类。  
  
- 仅当结构使用参数时，才可具有非共享构造函数;类可以具有或不带参数。  
  
 每个结构都具有一个不带参数的隐式公共构造函数。 此构造函数将结构的所有数据元素初始化为其默认值。 不能重新定义此行为。  
  
## <a name="instances-and-variables"></a>实例和变量  

 由于结构是值类型，因此每个结构变量将永久绑定到单个结构实例。 但类是引用类型，而对象变量可在不同时间引用各种类实例。 这种区分会按以下方式影响结构和类的使用：  
  
- **起始.** 结构变量使用结构的无参数构造函数隐式包括元素的初始化。 因此， `Dim s As struct1` 等效于 `Dim s As struct1 = New struct1()` 。  
  
- **分配变量。** 将一个结构变量分配给另一个结构变量或将结构实例传递给过程参数时，所有变量元素的当前值都将复制到新结构。 当您将一个对象变量分配给另一个对象变量，或将一个对象变量传递给某个过程时，只会复制引用指针。  
  
- **不分配任何内容。** 您可以将值 [Nothing](../../../language-reference/nothing.md) 赋给结构变量，但该实例继续与变量关联。 尽管变量元素由赋值重新初始化，仍可以调用其方法并访问其数据元素。  
  
     与此相反，如果您将一个对象变量设置为 `Nothing` ，则将其与任何类实例取消关联，并且您无法通过该变量访问任何成员，除非您向其分配了其他实例。  
  
- **多个实例。** 对象变量可在不同时间分配给它的不同类实例，而多个对象变量可同时引用相同的类实例。 对类成员的值所做的更改会在通过指向同一实例的另一个变量进行访问时影响这些成员。  
  
     但是，结构元素在其自己的实例中是隔离的。 对其值所做的更改不会反映在其他任何结构变量中，即使在同一声明的其他实例中也是如此 `Structure` 。  
  
- **等于.** 必须使用逐个元素测试来执行两个结构的相等性测试。 可以使用方法比较两个对象变量 <xref:System.Object.Equals%2A> 。 <xref:System.Object.Equals%2A> 指示两个变量是否指向同一个实例。  
  
## <a name="see-also"></a>请参阅

- [数据类型](index.md)
- [复合数据类型](composite-data-types.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [结构](structures.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [结构和其他编程元素](structures-and-other-programming-elements.md)
- [对象和类](../objects-and-classes/index.md)
