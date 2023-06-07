---
title: 类和结构 - C# 编程指南
description: 介绍了如何在 C# 中使用类和结构。
ms.date: 01/17/2016
helpviewer_keywords:
- structs [C#], about structs
- classes [C#], overview
- C# language, structs
- C# language, objects
- objects [C#]
- C# language, classes
ms.assetid: cc39dbda-8754-423e-b5b1-16a1db0734c0
ms.openlocfilehash: 55fc2cdda5b79847266a03800e6c31e6e78a55f7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203990"
---
# <a name="classes-and-structs-c-programming-guide"></a>类和结构（C# 编程指南）

类和结构是 .NET 通用类型系统的两种基本构造。 每种本质上都是一种数据结构，其中封装了同属一个逻辑单元的一组数据和行为。 数据和行为是类或结构的*成员*，包括方法、属性和事件等（此主题稍后将具体列举）。  
  
 类或结构声明类似于一张蓝图，用于在运行时创建实例或对象。 如果定义 `Person` 类或结构，那么 `Person` 就是类型名称。 如果声明和初始化 `Person` 类型的变量 `p`，那么 `p` 就是所谓的 `Person` 对象或实例。 可以创建同一 `Person` 类型的多个实例，每个实例都可以有不同的属性和字段值。  
  
 类是引用类型。 创建类的对象后，向其分配对象的变量仅保留对相应内存的引用。 将对象引用分配给新变量后，新变量会引用原始对象。 通过一个变量所做的更改将反映在另一个变量中，因为它们引用相同的数据。  
  
 结构是值类型。 创建结构时，向其分配结构的变量保留结构的实际数据。 将结构分配给新变量时，会复制结构。 因此，新变量和原始变量包含相同数据的副本（共两个）。 对一个副本所做的更改不会影响另一个副本。  
  
 一般来说，类用于对更复杂的行为或应在类对象创建后进行修改的数据建模。 结构最适用于所含大部分数据不得在结构创建后进行修改的小型数据结构。  
  
 有关详细信息，请参阅[类](./classes.md)、[对象](./objects.md)和[结构类型](../../language-reference/builtin-types/struct.md)。  
  
## <a name="example"></a>示例  

 在以下示例中，`ProgrammingGuide` 命名空间中的 `CustomClass` 有以下三个成员：实例构造函数、`Number` 属性和 `Multiply` 方法。 `Program` 类中的 `Main` 方法创建 `CustomClass` 的实例（对象），此对象的方法和属性可使用点表示法进行访问。
  
 [!code-csharp[csProgGuideObjects#1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/class1.cs#1)]  
  
## <a name="encapsulation"></a>封装  

 *封装*有时称为面向对象的编程的第一支柱或原则。 根据封装原则，类或结构可以指定自己的每个成员对外部代码的可访问性。 可以隐藏不得在类或程序集外部使用的方法和变量，以限制编码错误或恶意攻击发生的可能性。  
  
 有关类的详细信息，请参阅[类](./classes.md)和[对象](./objects.md)。  
  
### <a name="members"></a>成员  

 所有方法、字段、常量、属性和事件都必须在类型中进行声明；这些被称为类型的*成员*。 C# 没有全局变量或方法，这一点其他某些语言不同。 即使是程序的入口点 `Main` 方法，也必须在类或结构中进行声明。 下面列出了所有可以在类或结构中声明的各种成员。  
  
- [字段](./fields.md)  
  
- [常量](./constants.md)  
  
- [属性](./properties.md)  
  
- [方法](./methods.md)  
  
- [构造函数](./constructors.md)  
  
- [事件](../events/index.md)  
  
- [终结器](./destructors.md)  
  
- [索引器](../indexers/index.md)  
  
- [运算符](../../language-reference/operators/index.md)  
  
- [嵌套类型](./nested-types.md)  
  
### <a name="accessibility"></a>可访问性  

 一些方法和属性可供类或结构外部的代码（称为“*客户端代码*”）调用或访问。 另一些方法和属性只能在类或结构本身中使用。 请务必限制代码的可访问性，仅供预期的客户端代码进行访问。 使用访问修饰符 [public](../../language-reference/keywords/public.md)、[protected](../../language-reference/keywords/protected.md)、[internal](../../language-reference/keywords/internal.md)、[protected internal](../../language-reference/keywords/protected-internal.md)、[private](../../language-reference/keywords/private.md) 和 [private protected](../../language-reference/keywords/private-protected.md) 可指定类型及其成员对客户端代码的可访问性。 可访问性的默认值为 `private`。 有关详细信息，请参阅[访问修饰符](./access-modifiers.md)。  
  
### <a name="inheritance"></a>继承  

 类（而非结构）支持继承的概念。 派生自另一个类（基类）的类自动包含基类的所有公共、受保护和内部成员（其构造函数和终结器除外）。 有关详细信息，请参阅[继承](./inheritance.md)和[多态性](./polymorphism.md)。  
  
 可以将类声明为 [abstract](../../language-reference/keywords/abstract.md)，即一个或多个方法没有实现代码。 尽管抽象类无法直接实例化，但可以作为提供缺少实现代码的其他类的基类。 类还可以声明为 [sealed](../../language-reference/keywords/sealed.md)，以阻止其他类继承。 有关详细信息，请参阅[抽象类、密封类及类成员](./abstract-and-sealed-classes-and-class-members.md)。  
  
### <a name="interfaces"></a>接口  

 类和结构可以继承多个接口。 继承自接口意味着类型实现接口中定义的所有方法。 有关详细信息，请参阅[接口](../interfaces/index.md)。  
  
### <a name="generic-types"></a>泛型类型  

 类和结构可以使用一个或多个类型参数进行定义。 客户端代码在创建类型实例时提供类型。 例如，<xref:System.Collections.Generic> 命名空间中的 <xref:System.Collections.Generic.List%601> 类就是用一个类型参数进行定义的。 客户端代码创建 `List<string>` 或 `List<int>` 的实例来指定列表将包含的类型。 有关详细信息，请参阅[泛型](../generics/index.md)。  
  
### <a name="static-types"></a>静态类型  

 类（而非结构）可以声明为 [static](../../language-reference/keywords/static.md)。 静态类只能包含静态成员，不能使用新的关键字进行实例化。 在程序加载时，类的一个副本会加载到内存中，而其成员则可通过类名进行访问。 类和结构都能包含静态成员。 有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。  
  
### <a name="nested-types"></a>嵌套类型  

 类或结构可以嵌套在另一类或结构中。 有关详细信息，请参阅[嵌套类型](./nested-types.md)。  
  
### <a name="partial-types"></a>分部类型  

 可以在一个代码文件中定义类、结构或方法的一部分，并在其他代码文件中定义另一部分。 有关详细信息，请参阅[分部类和方法](./partial-classes-and-methods.md)。  
  
### <a name="object-initializers"></a>对象初始值设定项  

 无需显式调用构造函数，即可实例化和初始化类或结构对象和对象集合。 有关详细信息，请参阅[对象和集合初始值设定项](./object-and-collection-initializers.md)。  
  
### <a name="anonymous-types"></a>匿名类型  

 如果不方便或没有必要创建已命名的类（例如，使用无需保留或传递给其他方法的数据结构填充列表时），可以使用匿名类型。 有关详细信息，请参阅[匿名类型](./anonymous-types.md)。  
  
### <a name="extension-methods"></a>扩展方法  

 可以单独创建类型（其方法可以调用，就像它们属于原始类型一样）来“扩展”类，而无需创建派生类。 有关详细信息，请参阅[扩展方法](./extension-methods.md)。  
  
### <a name="implicitly-typed-local-variables"></a>隐式类型的局部变量  

 在类或结构方法中，可以使用隐式类型指示编译器在编译时确定正确的类型。 有关详细信息，请参阅[隐式类型局部变量](./implicitly-typed-local-variables.md)。  
  
## <a name="c-language-specification"></a>C# 语言规范  

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
