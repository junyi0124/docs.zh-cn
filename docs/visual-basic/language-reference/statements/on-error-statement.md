---
title: On Error 语句
ms.date: 07/20/2015
f1_keywords:
- vb.OnError
helpviewer_keywords:
- Visual Basic code, control flow
- Resume Next statement [Visual Basic]
- errors [Visual Basic], trapping
- error handling, On Error statement
- Next statement [Visual Basic], On Error
- control flow [Visual Basic], branching
- Error keyword [Visual Basic]
- execution [Visual Basic], conditional
- Resume statement [Visual Basic], and On Error statement
- Error statement [Visual Basic], and On Error statement
- GoTo statement [Visual Basic], and On Error statement
- branching [Visual Basic], on error
- conditional statements [Visual Basic], On Error
- On Error statement [Visual Basic], syntax
- On keyword [Visual Basic]
- run-time errors [Visual Basic], handling
- On Error statement [Visual Basic]
ms.assetid: ff947930-fb84-40cf-bd66-1ea219561d5c
ms.openlocfilehash: 7e007d59292fc577c0c8927766423ba6f7896a71
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873183"
---
# <a name="on-error-statement-visual-basic"></a>On Error 语句 (Visual Basic)

启用错误处理例程，并指定例程在过程中的位置;还可用于禁用错误处理例程。 `On Error`语句用在非结构化错误处理中，可以使用而不是结构化异常处理。 [结构化异常处理](../../../standard/exceptions/index.md) 内置于 .net 中，通常更高效，因此在处理应用程序中的运行时错误时建议使用。

 如果没有错误处理或异常处理，则发生的任何运行时错误都是致命的：将显示一条错误消息，并停止执行。

> [!NOTE]
> `Error`关键字还用于[Error 语句](error-statement.md)，该语句支持向后兼容性。

## <a name="syntax"></a>语法

```vb
On Error { GoTo [ line | 0 | -1 ] | Resume Next }
```

## <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`GoTo`*行*|启用从必需 *line* 参数所指定的行开始的错误处理例程。 *Line*参数是任意行标签或行号。 如果出现运行时错误，控制将分支到指定的行，使错误处理程序处于活动状态。 指定的行必须与语句位于同一过程中， `On Error` 否则将发生编译时错误。|
|`GoTo 0`|禁用当前过程中启用的错误处理程序，并将其重置为 `Nothing` 。|
|`GoTo -1`|在当前过程中禁用已启用的异常并将其重置为 `Nothing` 。|
|`Resume Next`|指定在发生运行时错误时，控制转到紧随发生错误的语句之后的语句，然后从该点继续执行。 在访问对象时，请使用此窗体而不是 `On Error GoTo` 。|

## <a name="remarks"></a>备注

> [!NOTE]
> 建议尽可能地在代码中使用结构化异常处理，而不是使用非结构化异常处理和 `On Error` 语句。 有关详细信息，请参阅 [Try...Catch...Finally 语句](try-catch-finally-statement.md)。

 "Enabled" 错误处理程序是指由语句打开的错误处理程序 `On Error` 。 "活动" 错误处理程序是在处理错误的过程中的已启用处理程序。

 如果错误处理程序处于活动状态时发生错误， (在发生错误的情况下出现错误 `Resume` ， `Exit Sub` `Exit Function`) ， `Exit Property` 则当前过程的错误处理程序将无法处理此错误。 控件返回到调用过程。
  
 如果调用过程具有已启用的错误处理程序，则会激活该处理程序来处理错误。 如果调用过程的错误处理程序也处于活动状态，控制将通过以前的调用过程返回，直到找到已启用但不活动的错误处理程序。 如果找不到这样的错误处理程序，则该错误在实际发生时是致命的。
  
 每次错误处理程序将控制权返回给调用过程时，该过程将成为当前过程。 一旦任何过程中的错误处理程序处理错误，就会在当前过程中的语句指定的点继续执行 `Resume` 。
  
> [!NOTE]
> 错误处理例程不是 `Sub` 过程或 `Function` 过程。 它是由行标签或行号标记的代码部分。
  
## <a name="number-property"></a>Number 属性

 错误处理例程依赖于对象的属性中的值 `Number` `Err` 来确定错误的原因。 在 `Err` 发生任何其他错误之前，或在调用可能导致错误的过程之前，例程应测试或保存对象中的相关属性值。 对象中的属性值 `Err` 仅反映最近的错误。 与关联的错误消息 `Err.Number` 包含在中 `Err.Description` 。  
  
## <a name="throw-statement"></a>Throw 语句  

 使用方法引发的错误会 `Err.Raise` 将 `Exception` 属性设置为类的新创建实例 <xref:System.Exception> 。 为了支持引发派生异常类型的异常， `Throw` 语言支持语句。 这需要一个参数，该参数是要引发的异常实例。 下面的示例演示如何将这些功能与现有的异常处理支持结合使用：

 [!code-vb[VbVbalrErrorHandling#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#17)]  
  
 请注意， `On Error GoTo` 语句捕获所有错误，而不考虑异常类。
  
## <a name="on-error-resume-next"></a>On Error Resume Next

 `On Error Resume Next` 使执行继续使用紧随导致运行时错误的语句之后的语句，或紧随包含语句的过程的最近调用之后的语句 `On Error Resume Next` 。 此语句允许在运行时错误的情况下继续执行。 可以放置错误处理例程，其中发生错误，而不是将控制转移到过程内的另一个位置。 `On Error Resume Next`当调用另一个过程时，语句变为非活动状态，因此， `On Error Resume Next` 如果您希望在该例程中进行内联错误处理，则应在每个调用的例程中执行语句。
  
> [!NOTE]
> 在 `On Error Resume Next` `On Error GoTo` 处理访问其他对象的过程中生成的错误时，构造可能更好。 `Err`在每次与对象交互后进行检查将消除代码所访问的对象的歧义。 您可以确定哪个对象放置了错误代码，以及 `Err.Number` 哪个对象最初生成了错误 (在) 中指定的对象 `Err.Source` 。

## <a name="on-error-goto-0"></a>On Error GoTo 0

 `On Error GoTo 0` 禁用当前过程中的错误处理。 即使过程包含编号为0的行，它也不会将第0行指定为错误处理代码的开头。 如果没有 `On Error GoTo 0` 语句，则在退出过程时，将自动禁用错误处理程序。

## <a name="on-error-goto--1"></a>Error GoTo 时-1

 `On Error GoTo -1` 禁用当前过程中的异常。 即使过程包含编号为-1 的行，它也不会将第1行指定为错误处理代码的开头。 如果不使用 `On Error GoTo -1` 语句，则在退出过程时，将自动禁用异常。

 若要防止错误处理代码在未发生错误时运行，请将 `Exit Sub` 、 `Exit Function` 或 `Exit Property` 语句紧靠在错误处理例程之前，如以下代码片段所示：

 [!code-vb[VbVbalrErrorHandling#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#18)]

 此处，错误处理代码将遵循 `Exit Sub` 语句，并在语句之前执行， `End Sub` 以将其与过程流区分开来。 可以在过程中的任何位置放置错误处理代码。

## <a name="untrapped-errors"></a>未捕获错误

 当对象作为可执行文件运行时，对象中的未捕获错误将返回给控制应用程序。 在开发环境中，仅当设置了正确的选项时，未捕获错误才能返回给控制应用程序。 请参阅宿主应用程序的文档，以了解在调试期间应设置哪些选项、如何设置这些选项以及宿主是否可以创建类。

 如果创建的对象访问其他对象，则应尝试处理它们传回的任何未处理的错误。 如果无法做到这一点，请将中的错误代码映射 `Err.Number` 到你自己的错误之一，然后将它们传递回你的对象的调用方。 应通过将错误代码添加到常量来指定错误 `VbObjectError` 。 例如，如果错误代码为1052，请按如下所示分配：

 [!code-vb[VbVbalrErrorHandling#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#19)]

> [!CAUTION]
> 调用 Windows 动态链接库 (Dll 时出现系统错误) 不会引发异常，并且在 Visual Basic 错误捕获时无法进行捕获。 调用 DLL 函数时，应根据 API 规范) 检查每个返回值的成功或失败 (，并在发生故障时检查 `Err` 对象的属性中的值 `LastDLLError` 。

## <a name="example"></a>示例

 此示例首先使用 `On Error GoTo` 语句在过程中指定错误处理例程的位置。 在此示例中，被零除的尝试将生成错误编号6。 错误在错误处理例程中进行处理，然后将控件返回给导致错误的语句。 `On Error GoTo 0`语句会关闭错误捕获。 然后， `On Error Resume Next` 使用语句将错误捕获延迟，以便对下一条语句所生成的错误上下文具有特定的已知上下文。 请注意，在 `Err.Clear` `Err` 处理错误后，用于清除对象的属性。

 [!code-vb[VbVbalrErrorHandling#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#20)]

## <a name="requirements"></a>要求

 **命名空间：** [Microsoft](../runtime-library-members.md)

 **程序集：** Microsoft.VisualBasic.dll) 中的 Visual Basic 运行时库 (

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Information.Err%2A>
- <xref:Microsoft.VisualBasic.ErrObject.Number%2A>
- <xref:Microsoft.VisualBasic.ErrObject.Description%2A>
- <xref:Microsoft.VisualBasic.ErrObject.LastDllError%2A>
- [End 语句](end-statement.md)
- [Exit 语句](exit-statement.md)
- [Resume 语句](resume-statement.md)
- [错误消息](../error-messages/index.md)
- [尝试 .。。Catch .。。Finally 语句](try-catch-finally-statement.md)
