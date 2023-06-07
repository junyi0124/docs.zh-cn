---
title: C++ 模板与 C# 泛型的区别 - C# 编程指南
description: 了解 C++ 模板和 C# 泛型之间的区别。 这两者都是支持参数化类型的语言功能。
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], vs. C++ templates
ms.assetid: 1da6beeb-d4a4-4da0-87b7-0cfbe04920b7
ms.openlocfilehash: 58b424e4dacd8b691c353f4eda1950f9710ef081
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167361"
---
# <a name="differences-between-c-templates-and-c-generics-c-programming-guide"></a>C++ 模板和 C# 泛型之间的区别（C# 编程指南）

C# 泛型和 C++ 模板均是支持参数化类型的语言功能。 但是，两者之间存在很多不同。 在语法层次，C# 泛型是参数化类型的一个更简单的方法，而不具有 C++ 模板的复杂性。 此外，C# 不试图提供  C++ 模板所具有的所有功能。 在实现层次，主要区别在于 C# 泛型类型的替换在运行时执行，从而为实例化对象保留了泛型类型信息。 有关详细信息，请参阅[运行时中的泛型](./generics-in-the-run-time.md)。  
  
 以下是 C# 泛型和 C++ 模板之间的主要差异：  
  
- C# 泛型的灵活性与 C++ 模板不同。 例如，虽然可以调用 C# 泛型类中的用户定义的运算符，但是无法调用算术运算符。  
  
- C# 不允许使用非类型模板参数，如 `template C<int i> {}`。  
  
- C# 不支持显式定制化；即特定类型模板的自定义实现。  
  
- C# 不支持部分定制化：部分类型参数的自定义实现。  
  
- C# 不允许将类型参数用作泛型类型的基类。  
  
- C# 不允许类型参数具有默认类型。  
  
- 在 C# 中，泛型类型参数本身不能是泛型，但是构造类型可以用作泛型。 C++ 允许使用模板参数。  
  
- C++ 允许在模板中使用可能并非对所有类型参数有效的代码，随后针对用作类型参数的特定类型检查此代码。 C# 要求类中编写的代码可处理满足约束的任何类型。 例如，在 C++ 中可以编写一个函数，此函数对类型参数的对象使用算术运算符 `+` 和 `-`，在实例化具有不支持这些运算符的类型的模板时，此函数将产生错误。 C# 不允许此操作；唯一允许的语言构造是可以从约束中推断出来的构造。  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [泛型介绍](./index.md)
- [模板](/cpp/cpp/templates-cpp)
