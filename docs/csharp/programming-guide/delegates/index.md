---
title: 委托 - C# 编程指南
description: C# 中的委托是一种类型，它引用具有参数列表和返回类型的方法。 委托用于将方法作为参数传递给其他方法。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, delegates
- delegates [C#]
ms.assetid: 97de039b-c76b-4b9c-a27d-8c1e1c8d93da
ms.openlocfilehash: 7365cb89ad617148fb26d5a01c07f13a7888bbf8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178692"
---
# <a name="delegates-c-programming-guide"></a>委托（C# 编程指南）

[委托](../../language-reference/builtin-types/reference-types.md)是一种引用类型，表示对具有特定参数列表和返回类型的方法的引用。 在实例化委托时，你可以将其实例与任何具有兼容签名和返回类型的方法相关联。 你可以通过委托实例调用方法。  
  
 委托用于将方法作为参数传递给其他方法。 事件处理程序就是通过委托调用的方法。 你可以创建一个自定义方法，当发生特定事件时，某个类（如 Windows 控件）就可以调用你的方法。 下面的示例演示了一个委托声明：  
  
 [!code-csharp[csProgGuideDelegates#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#20)]  
  
 可将任何可访问类或结构中与委托类型匹配的任何方法分配给委托。 该方法可以是静态方法，也可以是实例方法。 这样便能通过编程方式来更改方法调用，还可以向现有类中插入新代码。  
  
> [!NOTE]
> 在方法重载的上下文中，方法的签名不包括返回值。 但在委托的上下文中，签名包括返回值。 换句话说，方法和委托必须具有相同的返回类型。  
  
 将方法作为参数进行引用的能力使委托成为定义回调方法的理想选择。 例如，对比较两个对象的方法的引用可以作为参数传递到排序算法中。 由于比较代码在一个单独的过程中，因此可通过更常见的方式编写排序算法。  
  
## <a name="delegates-overview"></a>委托概述  

 委托具有以下属性：  
  
- 委托类似于 C++ 函数指针，但委托完全面向对象，不像 C++ 指针会记住函数，委托会同时封装对象实例和方法。
  
- 委托允许将方法作为参数进行传递。  
  
- 委托可用于定义回调方法。  
  
- 委托可以链接在一起；例如，可以对一个事件调用多个方法。  
  
- 方法不必与委托类型完全匹配。 有关详细信息，请参阅[使用委托中的变体](../concepts/covariance-contravariance/using-variance-in-delegates.md)。  
  
- C# 2.0 版引入了[匿名方法](../../language-reference/operators/delegate-operator.md)的概念，可以将代码块作为参数（而不是单独定义的方法）进行传递。 C# 3.0 引入了 Lambda 表达式，利用它们可以更简练地编写内联代码块。 匿名方法和 Lambda 表达式（在某些上下文中）都可编译为委托类型。 这些功能现在统称为匿名函数。 若要详细了解 lambda 表达式，请参阅 [lambda 表达式](../../language-reference/operators/lambda-expressions.md)。
  
## <a name="in-this-section"></a>本节内容  
  
- [使用委托](./using-delegates.md)  
  
- [何时使用委托，而不是接口（C# 编程指南）](/previous-versions/visualstudio/visual-studio-2010/ms173173(v=vs.100))  
  
- [带有命名方法的委托与带有匿名方法的委托](./delegates-with-named-vs-anonymous-methods.md)  
  
- [使用委托中的变体](../concepts/covariance-contravariance/using-variance-in-delegates.md)  
  
- [如何合并委托（多播委托）](./how-to-combine-delegates-multicast-delegates.md)  
  
- [如何声明、实例化和使用委托](./how-to-declare-instantiate-and-use-a-delegate.md)

## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[委托](~/_csharplang/spec/delegates.md)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="featured-book-chapters"></a>重要章节  

 [C# 3.0 手册（第三版）：面向 C# 3.0 程序员的超过 250 个解决方案](/previous-versions/visualstudio/visual-studio-2008/ff518995(v=orm.10))中的[委托、事件和 Lambda 表达式](/previous-versions/visualstudio/visual-studio-2008/ff518994(v=orm.10))  
  
 [学习 C# 3.0：掌握 C# 3.0 基础知识](/previous-versions/visualstudio/visual-studio-2008/ff652493(v=orm.10))中的[委托和事件](/previous-versions/visualstudio/visual-studio-2008/ff652490(v=orm.10))  
  
## <a name="see-also"></a>请参阅

- <xref:System.Delegate>
- [C# 编程指南](../index.md)
- [事件](../events/index.md)
