---
title: 对象 - C# 编程指南
description: C# 使用类或结构定义来定义对象的类型。 在 C# 等面向对象的语言中，程序由动态交互的对象组成。
ms.date: 07/20/2015
helpviewer_keywords:
- objects [C#], about objects
- variables [C#]
ms.assetid: af4a5230-fbf3-4eea-95e1-8b883c2f845c
ms.openlocfilehash: 61d79f5647fa05edade9aef90653544b08c20c83
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91181825"
---
# <a name="objects-c-programming-guide"></a>对象（C# 编程指南）

类或结构定义的作用类似于蓝图，指定该类型可以进行哪些操作。 从本质上说，对象是按照此蓝图分配和配置的内存块。 程序可以创建同一个类的多个对象。 对象也称为实例，可以存储在命名变量中，也可以存储在数组或集合中。 使用这些变量来调用对象方法及访问对象公共属性的代码称为客户端代码。 在 C# 等面向对象的语言中，典型的程序由动态交互的多个对象组成。  
  
> [!NOTE]
> 静态类型的行为与此处介绍的不同。 有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。
  
## <a name="struct-instances-vs-class-instances"></a>结构实例与类实例  

 由于类是引用类型，因此类对象的变量引用该对象在托管堆上的地址。 如果将同一类型的第二个对象分配给第一个对象，则两个变量都引用该地址的对象。 这一点将在本主题后面部分进行更详细的讨论。  
  
 类的实例是使用 [new 运算符](../../language-reference/operators/new-operator.md)创建的。 在下面的示例中，`Person` 为类型，`person1` 和 `person 2` 为该类型的实例（即对象）。  
  
 [!code-csharp[csProgGuideStatements#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#30)]  
  
 由于结构是值类型，因此结构对象的变量具有整个对象的副本。 结构的实例也可以使用 `new` 运算符来创建，但这不是必需的，如下面的示例所示：  
  
 [!code-csharp[csProgGuideStatements#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#31)]  
  
 `p1` 和 `p2` 的内存在线程堆栈上进行分配。 该内存随声明它的类型或方法一起回收。 这就是在赋值时复制结构的一个原因。 相比之下，当对类实例对象的所有引用都超出范围时，为该类实例分配的内存将由公共语言运行时自动回收（垃圾回收）。 无法像在 C++ 中那样明确地销毁类对象。 有关 .NET 中的垃圾回收的详细信息，请参阅[垃圾回收](../../../standard/garbage-collection/index.md)。  
  
> [!NOTE]
> 公共语言运行时中高度优化了托管堆上内存的分配和释放。 在大多数情况下，在堆上分配类实例与在堆栈上分配结构实例在性能成本上没有显著的差别。
  
## <a name="object-identity-vs-value-equality"></a>对象标识与值相等性  

 在比较两个对象是否相等时，首先必须明确是想知道两个变量是否表示内存中的同一对象，还是想知道这两个对象的一个或多个字段的值是否相等。 如果要对值进行比较，则必须考虑这两个对象是值类型（结构）的实例，还是引用类型（类、委托、数组）的实例。  
  
- 若要确定两个类实例是否引用内存中的同一位置（这意味着它们具有相同的标识），可使用静态 <xref:System.Object.Equals%2A> 方法。 （<xref:System.Object?displayProperty=nameWithType> 是所有值类型和引用类型的隐式基类，其中包括用户定义的结构和类。）  
  
- 若要确定两个结构实例中的实例字段是否具有相同的值，可使用 <xref:System.ValueType.Equals%2A?displayProperty=nameWithType> 方法。 由于所有结构都隐式继承自 <xref:System.ValueType?displayProperty=nameWithType>，因此可以直接在对象上调用该方法，如以下示例所示：  
  
 [!code-csharp[csProgGuideStatements#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#32)]  
  
 `Equals` 的 <xref:System.ValueType?displayProperty=nameWithType> 实现使用反射，因为它必须能够确定任何结构中有哪些字段。 在创建自己的结构时，重写 `Equals` 方法可以提供特定于你的类型的高效求等算法。  
  
- 若要确定两个类实例中字段的值是否相等，可以使用 <xref:System.Object.Equals%2A> 方法或 [== 运算符](../../language-reference/operators/equality-operators.md#equality-operator-)。 但是，只有类通过重写或重载提供关于那种类型对象的“相等”含义的自定义时，才能使用它们。 类也可能实现 <xref:System.IEquatable%601> 接口或 <xref:System.Collections.Generic.IEqualityComparer%601> 接口。 这两个接口都提供可用于测试值相等性的方法。 设计好替代 `Equals` 的类后，请务必遵循[如何为类型定义值相等性](../statements-expressions-operators/how-to-define-value-equality-for-a-type.md)和 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 中介绍的准则。
  
## <a name="related-sections"></a>相关章节  

 更多相关信息：  
  
- [类](./classes.md)  
  
- [构造函数](./constructors.md)  
  
- [终结器](./destructors.md)  
  
- [事件](../events/index.md)  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [object](../../language-reference/builtin-types/reference-types.md)
- [继承](./inheritance.md)
- [class](../../language-reference/keywords/class.md)
- [结构类型](../../language-reference/builtin-types/struct.md)
- [new 运算符](../../language-reference/operators/new-operator.md)
- [常规类型系统](../../../standard/base-types/common-type-system.md)
