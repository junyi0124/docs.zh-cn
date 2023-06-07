---
title: 错误类型
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions, types
- errors [Visual Basic], types
- errors [Visual Basic], logic
- errors [Visual Basic], syntax
- logic errors [Visual Basic], Visual Basic
- run-time errors [Visual Basic], types of errors
- syntax errors [Visual Basic], Visual Basic
ms.assetid: 3048aabf-8c97-4e13-9150-853769cb5f6f
ms.openlocfilehash: caeaab9a358e3e3a995c1df7274d16daaff7a667
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91083990"
---
# <a name="error-types-visual-basic"></a>错误类型 (Visual Basic)

在 Visual Basic 中，错误分为三个类别之一：语法错误、运行时错误和逻辑错误。

## <a name="syntax-errors"></a>语法错误

 *语法错误* 是在编写代码时显示的错误。 如果使用的是 Visual Studio，则 Visual Basic 在 **代码编辑器** 窗口中键入代码时检查代码，并在出现错误时发出警报，如错误拼写错误或错误地使用语言元素。 如果从命令行进行编译，Visual Basic 将显示编译器错误，其中包含有关语法错误的信息。 语法错误是最常见的错误类型。 一旦出现，就可以在编码环境中轻松地对其进行修复。

> [!NOTE]
> `Option Explicit`语句是避免语法错误的一种方法。 它强制您事先声明要在应用程序中使用的所有变量。 因此，当在代码中使用这些变量时，会立即捕获任何排字错误，并可修复这些错误。

## <a name="run-time-errors"></a>运行时错误

 *运行时错误* 是指仅在编译和运行代码后显示的错误。 它们涉及的代码可能看起来是正确的，因为它没有语法错误，但不会执行。 例如，你可能会正确编写一行代码来打开文件。 但是，如果该文件不存在，应用程序将无法打开该文件，并将引发异常。 您可以通过重写错误的代码或使用 [异常处理](../../language-reference/statements/try-catch-finally-statement.md)来修复大多数运行时错误，然后重新编译并重新运行它。
  
## <a name="logic-errors"></a>逻辑错误

 *逻辑错误* 是指在应用程序使用后出现的错误。 它们最常见的是开发人员作出的假设，或者是不需要或意外的结果来响应用户操作。 例如，键入错误的键可能会为方法提供错误的信息，或者，如果不是这样，则您可能会假设始终向方法提供有效的值。 尽管可以使用 [异常处理](../../language-reference/statements/try-catch-finally-statement.md) 来处理逻辑错误 (例如，通过测试参数是否为 `Nothing` 并引发 <xref:System.ArgumentNullException>) ，最常见的解决方法是更正逻辑中的错误并重新编译应用程序。

## <a name="see-also"></a>请参阅

- [Try...Catch...Finally 语句](../../language-reference/statements/try-catch-finally-statement.md)
- [调试器基础知识](/visualstudio/debugger/debugger-feature-tour)
