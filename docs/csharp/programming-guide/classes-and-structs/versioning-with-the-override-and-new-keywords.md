---
title: 使用 Override 和 New 关键字进行版本控制 - C# 编程指南
description: 了解 C# 中的基类和派生类的版本控制，并了解如何指定某方法是要替代一个继承方法还是要隐藏一个继承方法。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, versioning
- C# language, override and new
ms.assetid: 88247d07-bd0d-49e9-a619-45ccbbfdf0c5
ms.openlocfilehash: 13321cdc83637105a2b981902ce984e6a90a25d9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91181812"
---
# <a name="versioning-with-the-override-and-new-keywords-c-programming-guide"></a>使用 Override 和 New 关键字进行版本控制（C# 编程指南）

C# 语言经过专门设计，以便不同库中的[基类](../../language-reference/keywords/base.md)与派生类之间的版本控制可以不断向前发展，同时保持后向兼容。 这具有多方面的意义。例如，这意味着在基[类](../../language-reference/keywords/class.md)中引入与派生类中的某个成员具有相同名称的新成员在 C# 中是完全支持的，不会导致意外行为。 它还意味着类必须显式声明某方法是要替代一个继承方法，还是本身就是一个隐藏具有类似名称的继承方法的新方法。  
  
 在 C# 中，派生类可以包含与基类方法同名的方法。  

- 如果派生类中的方法前面没有 [new](../../language-reference/keywords/new-modifier.md) 或 [override](../../language-reference/keywords/override.md) 关键字，则编译器将发出警告，该方法将如同存在 `new` 关键字一样执行操作。  
  
- 如果派生类中的方法前面带有 `new` 关键字，则该方法被定义为独立于基类中的方法。  
  
- 如果派生类中的方法前面带有 `override` 关键字，则派生类的对象将调用该方法，而不是调用基类方法。  

- 若要将 `override` 关键字应用于派生类中的方法，必须以[虚拟](../../language-reference/keywords/virtual.md)形式定义基类方法。
  
- 可以从派生类中使用 `base` 关键字调用基类方法。  
  
- `override`、`virtual` 和 `new` 关键字还可以用于属性、索引器和事件中。  
  
 默认情况下，C# 方法为非虚方法。 如果某个方法被声明为虚方法，则继承该方法的任何类都可以实现它自己的版本。 若要使方法成为虚方法，需要在基类的方法声明中使用 `virtual` 修饰符。 然后，派生类可以使用 `override` 关键字替代基虚方法，或使用 `new` 关键字隐藏基类中的虚方法。 如果 `override` 关键字和 `new` 关键字均未指定，编译器将发出警告，并且派生类中的方法将隐藏基类中的方法。  
  
 为了在实践中演示上述情况，暂时假定公司 A 创建了一个名为 `GraphicsClass` 的类，程序将使用此类。 `GraphicsClass` 如下所示：  
  
 [!code-csharp[csProgGuideInheritance#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#27)]  
  
 公司使用此类，并且你在添加新方法时将其用于派生自己的类：  
  
 [!code-csharp[csProgGuideInheritance#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#28)]  
  
 你的应用程序运行正常，未出现问题，直到公司 A 发布了 `GraphicsClass` 的新版本，类似于以下代码：  
  
 [!code-csharp[csProgGuideInheritance#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#29)]  
  
 现在，`GraphicsClass` 的新版本中包含一个名为 `DrawRectangle` 的方法。 开始时，没有出现任何问题。 新版本仍然与旧版本保持二进制兼容。 已经部署的任何软件都将继续正常工作，即使新类已安装到这些计算机系统上。 在你的派生类中，对方法 `DrawRectangle` 的任何现有调用将继续引用你的版本。  
  
 但是，一旦你使用 `GraphicsClass` 的新版本重新编译应用程序，就会收到来自编译器的警告 CS0108。 此警告提示，必须考虑你所期望的 `DrawRectangle` 方法在应用程序中的工作方式。  
  
 如果需要自己的方法替代新的基类方法，请使用 `override` 关键字：  
  
 [!code-csharp[csProgGuideInheritance#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#30)]  
  
 `override` 关键字可确保派生自 `YourDerivedGraphicsClass` 的任何对象都将使用 `DrawRectangle` 的派生类版本。 派生自 `YourDerivedGraphicsClass` 的对象仍可以使用 base 关键字访问 `DrawRectangle` 的基类版本：  
  
 [!code-csharp[csProgGuideInheritance#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#44)]  
  
 如果不需要自己的方法替代新的基类方法，则需要注意以下事项。 为了避免这两个方法之间发生混淆，可以重命名你的方法。 这可能很耗费时间且容易出错，而且在某些情况下并不可行。 但是，如果项目相对较小，则可以使用 Visual Studio 的重构选项来重命名方法。 有关详细信息，请参阅[重构类和类型（类设计器）](/visualstudio/ide/class-designer/refactoring-classes-and-types)。  
  
 或者，也可以通过在派生类定义中使用关键字 `new` 来防止出现该警告：  
  
 [!code-csharp[csProgGuideInheritance#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#31)]  
  
 使用 `new` 关键字可告诉编译器你的定义将隐藏基类中包含的定义。 这是默认行为。  
  
## <a name="override-and-method-selection"></a>替代和方法选择  

 当在类中对方法进行命名时，如果有多个方法与调用兼容（例如，存在两种同名的方法，并且其参数与传递的参数兼容），则 C# 编译器将选择最佳方法进行调用。 以下方法将是兼容的：  
  
 [!code-csharp[csProgGuideInheritance#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#32)]  
  
 在 `Derived` 的一个实例中调用 `DoWork` 时，C# 编译器将首先尝试使该调用与最初在 `Derived` 上声明的 `DoWork` 版本兼容。 替代方法不被视为是在类上进行声明的，而是在基类上声明的方法的新实现。 仅当 C# 编译器无法将方法调用与 `Derived` 上的原始方法匹配时，才尝试将该调用与具有相同名称和兼容参数的替代方法匹配。 例如：  
  
 [!code-csharp[csProgGuideInheritance#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#33)]  
  
 由于变量 `val` 可以隐式转换为 double 类型，因此 C# 编译器将调用 `DoWork(double)`，而不是 `DoWork(int)`。 有两种方法可以避免此情况。 首先，避免将新方法声明为与虚方法相同的名称。 其次，可以通过将 `Derived` 的实例强制转换为 `Base` 来使 C# 编译器搜索基类方法列表，从而使其调用虚方法。 由于是虚方法，因此将调用 `Derived` 上的 `DoWork(int)` 的实现。 例如：  
  
 [!code-csharp[csProgGuideInheritance#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#34)]  
  
 有关 `new` 和 `override` 的更多示例，请参阅[了解何时使用 Override 和 New 关键字](./knowing-when-to-use-override-and-new-keywords.md)。  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类和结构](./index.md)
- [方法](./methods.md)
- [继承](./inheritance.md)
