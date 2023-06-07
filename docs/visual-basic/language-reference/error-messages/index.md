---
title: Visual Basic 错误消息
titleSuffix: ''
ms.date: 10/13/2020
helpviewer_keywords:
- errors [Visual Basic]
- error messages
ms.assetid: f2dda05b-baef-41f5-8bb1-598bd7cf239f
ms.openlocfilehash: 70973b6d34ed1a84a38af3694e144dfcefa61c20
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163371"
---
# <a name="error-messages-in-visual-basic"></a>Visual Basic 中的错误消息

在编译或运行 Visual Basic 应用程序时，可能会出现以下类型的错误：

- 编译时错误，在编译应用程序时发生。

- 运行时错误，在应用程序运行时发生。

若要了解如何排查特定错误，请参阅[为 Visual Basic 程序员提供的附加资源](../../getting-started/additional-resources.md)。

## <a name="run-time-errors"></a>运行时错误

如果 Visual Basic 应用程序尝试执行系统无法执行的操作，则会发生运行时错误，并且 Visual Basic 会引发 <xref:System.Exception> 对象。 Visual Basic 可以通过使用语句，生成任何数据类型（包括对象）的自定义错误 `Exception` `Throw` 。 应用程序可以通过显示捕获到的异常的错误号和消息来识别错误。 如果未捕获到错误，应用程序会结束。

代码可用于捕获和检查运行时错误。 如果将生成错误的代码封闭在 `Try` 代码块中，则可以在匹配的 `Catch` 代码块中捕获抛出的任何错误。 若要了解如何在运行时捕获错误并在代码中响应错误，请参阅 [Try...Catch...Finally 语句](../statements/try-catch-finally-statement.md)。

## <a name="compile-time-errors"></a>编译时错误

如果 Visual Basic 编译器遇到代码问题，则会发生编译时错误。 在 Visual Studio 代码编辑器中，你可以轻松地识别导致错误的代码行，因为该代码行下出现一条波浪线。 指向波形下划线或打开“错误列表”****，即可看到错误消息（还可以查看其他消息）。

如果标识符具有波浪形下划线并且在最右边的字符下出现短下划线，则可以为类、构造函数、方法、属性、字段或枚举生成存根。 有关详细信息，请参阅 [使用 Visual Studio (生成) ](/visualstudio/ide/visual-csharp-intellisense#generate-from-usage)。

通过解决 Visual Basic 编译器警告提示的问题，可以编写运行速度更快、bug 更少的代码。 这些警告可以识别可能会在应用程序运行时生成错误的代码。 例如，如果尝试调用未赋值的对象变量的成员、让未设置返回值的函数返回值或执行有逻辑错误的 `Try` 代码块来捕获异常，编译器都会生成警告。 若要详细了解警告（包括如何启用和禁用警告），请参阅[在 Visual Basic 中配置警告](/visualstudio/ide/configuring-warnings-in-visual-basic)。
