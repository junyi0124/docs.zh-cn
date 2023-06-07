---
title: F# 样式指南
description: 了解理想的 F# 代码的五个原则。
ms.date: 12/10/2018
ms.openlocfilehash: 9f47257626e04b09b546de2ae315d48d791678be
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223649"
---
# <a name="f-style-guide"></a>F# 样式指南

下文介绍了有关设置 F# 代码格式的准则，以及有关该语言功能及其使用方式的主题指南。

本指南是基于在具有不同程序员组的大型代码库中使用 F# 而制定的。 本指南通常可引导你成功使用 F#，并在项目需求随时间变化时最大限度地降低挫折感。

## <a name="five-principles-of-good-f-code"></a>理想的 F# 代码的五个原则

无论何时编写 F# 代码（尤其是在随时间变化的系统中），请始终牢记以下原则。 后续文章中的每一条准则都源于这五点。

1. **理想的 F# 代码具有简洁、表达力强且可组合的特点**

    F# 提供了许多功能，使你能够通过更少的代码行表达操作和重复使用一般性功能。 F# 核心库还包含许多有用的类型和函数，用于处理常见的数据集合。 组合你自己的函数和 F# 核心库（或其他库）中的函数是日常惯用的 F# 编程的一部分。 一般规则是，若能使用更少的代码行表达问题的解决方案，则对于其他开发人员（或未来你自己）会很有帮助。 同时，强烈建议使用库，如 FSharp.Core，它是 F# 在其上运行的[大型 .NET 库](../../../api/index.md)，或在需要执行重要任务时使用 [NuGet](https://www.nuget.org/) 上的第三方包。

2. **理想的 F# 代码应具备互操作性**

    互操作性可以采用多种形式，包括使用不同语言的代码。 其他调用方与之互操作的代码边界是正确处理的关键部分，即使调用方使用的也是 F#。 编写 F# 时，应始终考虑其他代码将如何调用你正在编写的代码，包括它们是否通过另一种语言（如 C#）调用它们。 [F# 组件设计准则](component-design-guidelines.md)详细介绍互操作性。

3. **理想的 F# 代码利用对象编程，而不是面向对象**

    F# 完全支持使用 .NET 中的对象进行编程，其中包括[类](../language-reference/classes.md)、[接口](../language-reference/interfaces.md)、[访问修饰符](../language-reference/access-control.md)、[抽象类](../language-reference/abstract-classes.md)等。 对于更复杂的函数代码（如必须识别上下文的函数），对象可以轻松地以函数无法实现的方式封装上下文信息。 借助[可选参数](../language-reference/members/methods.md#optional-arguments)和谨慎使用[重载](../language-reference/members/methods.md#overloaded-methods)等功能，调用者可以更容易地使用此功能。

4. **理想的 F# 代码执行良好，无需公开变异**

    众所周知，要编写高性能代码，必须使用变异。 因为这就是计算机的工作方式。 此类代码通常很容易出错，而且很难纠正。 避免向调用方公开变异。 相反，[生成一个功能接口，用于在性能至关重要时隐藏基于变异的实现](conventions.md#performance)。

5. **理想的 F# 代码易于使用工具处理**

    工具对于在大型代码库中工作非常重要，你可以编写 F# 代码，使其更有效地与 F# 语言工具结合使用。 其中的一个示例是，确保你没有过度使用无参数编程风格，这样就可以用调试程序检查中间值。 另一个示例是使用 [XML 文档注释](../language-reference/xml-documentation.md)描述构造，以便编辑器中的工具提示可以在调用站点显示这些注释。 请始终考虑其他程序员如何使用其工具来读取、导航、调试和操作你的代码。

## <a name="next-steps"></a>后续步骤

[F# 代码格式设置准则](formatting.md)提供有关如何设置代码格式以便于阅读的指南。

[F# 编码约定](conventions.md)提供有关 F# 编程惯例的指南，有助于长期维护更大型的 F# 代码库。

[F# 组件设计准则](component-design-guidelines.md)提供有关设计 F# 组件（如库）的指南。
