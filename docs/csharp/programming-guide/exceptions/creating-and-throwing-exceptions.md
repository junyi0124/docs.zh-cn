---
title: 创建和抛出异常 - C# 编程指南
description: 了解如何创建和引发异常。 异常用于指示在运行程序时发生了错误。
ms.date: 12/09/2020
helpviewer_keywords:
- catching exceptions [C#]
- throwing exceptions [C#]
- exceptions [C#], creating
- exceptions [C#], throwing
ms.assetid: 6bbba495-a115-4c6d-90cc-1f4d7b5f39e2
ms.openlocfilehash: a6998c5b274736b290460808ad4c7cb22be7ebf6
ms.sourcegitcommit: 9b877e160c326577e8aa5ead22a937110d80fa44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97110203"
---
# <a name="creating-and-throwing-exceptions-c-programming-guide"></a>创建和引发异常（C# 编程指南）

异常用于指示在运行程序时发生了错误。 此时将创建一个描述错误的异常对象，然后使用 [throw](../../language-reference/keywords/throw.md) 关键字引发。 然后，运行时搜索最兼容的异常处理程序。

当存在下列一种或多种情况时，程序员应引发异常：

- 方法无法完成其定义的功能。 例如，如果一种方法的参数具有无效的值：
  :::code language="csharp" source="snippets/exceptions/Program.cs" ID="CantComplete":::
- 根据对象的状态，对某个对象进行不适当的调用。 一个示例可能是尝试写入只读文件。 在对象状态不允许操作的情况下，引发 <xref:System.InvalidOperationException> 的实例或基于此类的派生的对象。 以下代码是引发 <xref:System.InvalidOperationException> 对象的方法示例：
  :::code language="csharp" source="snippets/exceptions/ProgramLog.cs" ID="ProgramLog":::
- 方法的参数引发了异常。 在这种情况下，应捕获原始异常，并创建 <xref:System.ArgumentException> 实例。 应将原始异常作为 <xref:System.Exception.InnerException%2A> 参数传递给 <xref:System.ArgumentException> 的构造函数：
  :::code language="csharp" source="snippets/exceptions/Program.cs" ID="InvalidArg":::

异常包含一个名为 <xref:System.Exception.StackTrace%2A> 的属性。 此字符串包含当前调用堆栈上的方法的名称，以及为每个方法引发异常的位置（文件名和行号）。 <xref:System.Exception.StackTrace%2A> 对象由公共语言运行时 (CLR) 从 `throw` 语句的位置点自动创建，因此必须从堆栈跟踪的开始点引发异常。

所有异常都包含一个名为 <xref:System.Exception.Message%2A> 的属性。 应设置此字符串来解释发生异常的原因。 不应将安全敏感的信息放在消息文本中。 除 <xref:System.Exception.Message%2A> 以外，<xref:System.ArgumentException> 也包含一个名为 <xref:System.ArgumentException.ParamName%2A> 的属性，应将该属性设置为导致引发异常的参数的名称。 在属性资源库中，<xref:System.ArgumentException.ParamName%2A> 应设置为 `value`。

公共的受保护方法在无法完成其预期功能时将引发异常。 引发的异常类是符合错误条件的最具体的可用异常。 这些异常应编写为类功能的一部分，并且原始类的派生类或更新应保留相同的行为以实现后向兼容性。

## <a name="things-to-avoid-when-throwing-exceptions"></a>引发异常时应避免的情况

以下列表标识了引发异常时要避免的做法：

- 不要使用异常在正常执行过程中更改程序的流。 使用异常来报告和处理错误条件。
- 只能引发异常，而不能作为返回值或参数返回异常。
- 请勿有意从自己的源代码中引发 <xref:System.Exception?displayProperty=nameWithType>、<xref:System.SystemException?displayProperty=nameWithType>、<xref:System.NullReferenceException?displayProperty=nameWithType> 或 <xref:System.IndexOutOfRangeException?displayProperty=nameWithType>。
- 不要创建可在调试模式下引发，但不会在发布模式下引发的异常。 若要在开发阶段确定运行时错误，请改用调试断言。

## <a name="defining-exception-classes"></a>定义异常类

程序可以引发 <xref:System> 命名空间中的预定义异常类（前面提到的情况除外），或通过从 <xref:System.Exception> 派生来创建其自己的异常类。 派生类应该至少定义四个构造函数：一个无参数构造函数、一个用于设置消息属性，还有一个用于设置 <xref:System.Exception.Message%2A> 和 <xref:System.Exception.InnerException%2A> 属性。 第四个构造函数用于序列化异常。 新的异常类应可序列化。 例如：

:::code language="csharp" source="snippets/exceptions/InvalidDepartmentException.cs" id="DefineExceptionClass":::

当新属性提供的数据有助于解决异常时，将新属性添加到异常类中。 如果将新属性添加到派生异常类中，则应替代 `ToString()` 以返回添加的信息。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[异常](~/_csharplang/spec/exceptions.md)和 [throw 语句](~/_csharplang/spec/statements.md#the-throw-statement)。 该语言规范是 C# 语法和用法的权威资料。

## <a name="see-also"></a>请参阅

- [异常层次结构](../../../standard/exceptions/index.md)
