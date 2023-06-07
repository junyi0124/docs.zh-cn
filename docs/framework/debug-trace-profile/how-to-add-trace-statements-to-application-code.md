---
title: 如何：向应用程序代码添加跟踪语句
description: 了解如何在 .NET 中将跟踪语句添加到应用程序代码。 跟踪使用最常用的方法是将输出写入侦听器的方法。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework], conditional writes based on switches
- trace statements
- WriteLineIf method
- tracing [.NET Framework], adding trace statements
- Assert method, tracing code
- trace switches, conditional writes based on switches
- WriteIf method
ms.assetid: f3a93fa7-1717-467d-aaff-393e5c9828b4
ms.openlocfilehash: 6beecf39d4372a194a9110ed8942b998443934d4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244205"
---
# <a name="how-to-add-trace-statements-to-application-code"></a>如何：向应用程序代码添加跟踪语句

最常用于跟踪的方法是用于将输出写入侦听器的以下方法：Write、WriteIf、WriteLine、WriteLineIf、Assert 和 Fail。 这些方法可以分为两类：Write、WriteLine 和 Fail 都无条件地发出输出，而 WriteIf、WriteLineIf 和 Assert 则测试 Boolean 条件并根据条件的值来写入或不写入。 WriteIf 和 WriteLineIf 在条件为 `true` 时发出输出，而 Assert 在条件为 `false` 时发出输出。  
  
 当设计跟踪和调试策略时，应考虑所需的输出形式。 使用不相关的信息填充的多个 **Write** 语句将创建难以阅读的日志。 另一方面，使用 **WriteLine** 将相关语句放置在单独的行上可能会很难区分哪些信息属于同一信息。 通常，当您想要将来自多个源的信息组合起来创建单个信息性消息时，请使用多个 **Write** 语句，并在需要创建单个完整消息时使用 **WriteLine** 语句。  
  
### <a name="to-write-a-complete-line"></a>写入完整的行  
  
1. 调用 <xref:System.Diagnostics.Trace.WriteLine%2A> 或 <xref:System.Diagnostics.Trace.WriteLineIf%2A> 方法。  
  
     一个回车符会被追加到此方法返回的消息末尾，使 Write、WriteIf、WriteLine 或 WriteLineIf 返回的下一条消息将从以下行开始：  
  
    ```vb  
    Dim errorFlag As Boolean = False  
    Trace.WriteLine("Error in AppendData procedure.")  
    Trace.WriteLineIf(errorFlag, "Error in AppendData procedure.")  
    ```  
  
    ```csharp  
    bool errorFlag = false;  
    System.Diagnostics.Trace.WriteLine ("Error in AppendData procedure.");  
    System.Diagnostics.Trace.WriteLineIf(errorFlag,
       "Error in AppendData procedure.");  
    ```  
  
### <a name="to-write-a-partial-line"></a>写入部分行  
  
1. 调用 <xref:System.Diagnostics.Trace.Write%2A> 或 <xref:System.Diagnostics.Trace.WriteIf%2A> 方法。  
  
     由 Write、WriteIf、WriteLine 或 WriteLineIf 生成的下一条消息将从由 Write 或 WriteIf 语句生成的消息所在的同一行上开始：  
  
    ```vb  
    Dim errorFlag As Boolean = False  
    Trace.WriteIf(errorFlag, "Error in AppendData procedure.")  
    Debug.WriteIf(errorFlag, "Transaction abandoned.")  
    Trace.Write("Invalid value for data request")  
    ```  
  
    ```csharp  
    bool errorFlag = false;  
    System.Diagnostics.Trace.WriteIf(errorFlag,
       "Error in AppendData procedure.");  
    System.Diagnostics.Debug.WriteIf(errorFlag, "Transaction abandoned.");  
    Trace.Write("Invalid value for data request");  
    ```  
  
### <a name="to-verify-that-certain-conditions-exist-either-before-or-after-you-execute-a-method"></a>验证特定条件在执行方法之前或之后存在  
  
1. 调用 <xref:System.Diagnostics.Trace.Assert%2A> 方法。  
  
    ```vb  
    Dim i As Integer = 4  
    Trace.Assert(i = 5, "i is not equal to 5.")  
    ```  
  
    ```csharp  
    int i = 4;  
    System.Diagnostics.Trace.Assert(i == 5, "i is not equal to 5.");  
    ```  
  
    > [!NOTE]
    > 可以将 Assert 用于跟踪和调试。 此示例将调用堆栈输出到 Listeners 集合中的任意侦听器。 有关详细信息，请参阅[托管代码中的断言<xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>和 ](/visualstudio/debugger/assertions-in-managed-code)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.Debug.WriteIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Debug.WriteLineIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Trace.WriteIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Trace.WriteLineIf%2A?displayProperty=nameWithType>
- [跟踪应用程序和在应用程序中插入检测点](tracing-and-instrumenting-applications.md)
- [如何：创建、初始化和配置跟踪开关](how-to-create-initialize-and-configure-trace-switches.md)
- [跟踪开关](trace-switches.md)
- [跟踪侦听器](trace-listeners.md)
