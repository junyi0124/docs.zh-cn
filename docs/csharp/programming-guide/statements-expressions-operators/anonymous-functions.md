---
title: 匿名函数 - C# 编程指南
description: 了解匿名函数。 可以使用 lambda 表达式或匿名方法来创建匿名函数。
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [C#], as anonymous functions
- anonymous functions [C#]
- anonymous methods [C#]
ms.assetid: 6ce3f04d-0c71-4728-9127-634c7e9a8365
ms.openlocfilehash: 1fde7d535054f09d55018a010468776622ebfba7
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063258"
---
# <a name="anonymous-functions-c-programming-guide"></a>匿名函数 -（C# 编程指南）

匿名函数是一个“内联”语句或表达式，可在需要委托类型的任何地方使用。 可以使用匿名函数来初始化命名委托，或传递命名委托（而不是命名委托类型）作为方法参数。

可以使用 [lambda 表达式](../../language-reference/operators/lambda-expressions.md)或[匿名方法](../../language-reference/operators/delegate-operator.md)来创建匿名函数。 建议使用 lambda 表达式，因为它们提供了更简洁和富有表现力的方式来编写内联代码。 与匿名方法不同，某些类型的 lambda 表达式可以转换为表达式树类型。

## <a name="the-evolution-of-delegates-in-c"></a>C\# 中委托的演变

 在 C# 1.0 中，通过使用在代码中其他位置定义的方法显式初始化委托来创建委托的实例。 C# 2.0 引入了匿名方法的概念，作为一种编写可在委托调用中执行的未命名内联语句块的方式。 C# 3.0 引入了 lambda 表达式，这种表达式与匿名方法的概念类似，但更具表现力并且更简练。 这两个功能统称为*匿名函数*。 通常，面向 .NET Framework 3.5 或更高版本的应用程序应使用 lambda 表达式。  
  
 下面的示例演示从 C# 1.0 到 C# 3.0 委托创建过程的发展：  
  
 [!code-csharp[csProgGuideLINQ#65](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#65)]  
  
## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [匿名函数表达式](~/_csharplang/spec/expressions.md#anonymous-function-expressions)部分。
  
## <a name="see-also"></a>请参阅

- [语句、表达式和运算符](./index.md)
- [Lambda 表达式](../../language-reference/operators/lambda-expressions.md)
- [委托](../delegates/index.md)
- [表达式树 (C#)](../concepts/expression-trees/index.md)
