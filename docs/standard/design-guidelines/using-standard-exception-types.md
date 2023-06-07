---
title: 使用标准异常类型
description: 了解 .NET 中的标准异常类型。 了解 SystemException、ApplicationException、ArgumentException、ComException 等。
ms.date: 10/22/2008
helpviewer_keywords:
- throwing exceptions, standard types
- catching exceptions
- exceptions, catching
- exceptions, throwing
ms.assetid: ab22ce03-78f9-4dca-8824-c7ed3bdccc27
ms.openlocfilehash: ef420d47e6204aef5e3d9bc12ace31fbf5521ee7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734356"
---
# <a name="using-standard-exception-types"></a>使用标准异常类型

本部分介绍框架提供的标准异常及其用法的详细信息。 列表并不完整。 请参阅 .NET Framework 参考文档，以了解其他框架异常类型的用法。

## <a name="exception-and-systemexception"></a>Exception 和 SystemException

 ❌ 不要引发 <xref:System.Exception?displayProperty=nameWithType> 或 <xref:System.SystemException?displayProperty=nameWithType> 。

 ❌ 不要捕获 `System.Exception` `System.SystemException` 框架代码中的或，除非你打算重新引发。

 ❌ 避免捕获 `System.Exception` 或 `System.SystemException` （顶级异常处理程序除外）。

## <a name="applicationexception"></a>ApplicationException

 ❌ 不要引发或派生自 <xref:System.ApplicationException> 。

## <a name="invalidoperationexception"></a>InvalidOperationException

 <xref:System.InvalidOperationException>如果对象处于不适当的状态，✔️会引发。

## <a name="argumentexception-argumentnullexception-and-argumentoutofrangeexception"></a>ArgumentException、System.argumentnullexception 和 ArgumentOutOfRangeException

 <xref:System.ArgumentException>如果向成员传递了错误的参数，✔️将引发或其子类型之一。 如果适用，更喜欢派生程度最高的异常类型。

 ✔️ `ParamName` 在引发的子类之一时设置属性 `ArgumentException` 。

 此属性表示导致引发异常的参数的名称。 请注意，可以使用构造函数重载之一来设置属性。

 ✔️用于 `value` 属性 setter 的隐式值参数的名称。

## <a name="nullreferenceexception-indexoutofrangeexception-and-accessviolationexception"></a>NullReferenceException、IndexOutOfRangeException 和 AccessViolationException

 ❌ 不要允许可公开调用的 Api 显式或隐式引发 <xref:System.NullReferenceException> 、 <xref:System.AccessViolationException> 或 <xref:System.IndexOutOfRangeException> 。 这些异常由执行引擎保留并引发，在大多数情况下表示 bug。

 执行参数检查以避免引发这些异常。 引发这些异常会公开方法的实现详细信息，这些详细信息可能会随时间变化。

## <a name="stackoverflowexception"></a>StackOverflowException

 ❌ 不要显式引发 <xref:System.StackOverflowException> 。 异常只应由 CLR 显式引发。

 ❌ 请勿捕获 `StackOverflowException` 。

 几乎无法编写在存在任意堆栈溢出时保持一致的托管代码。 CLR 的非托管部分保持一致，方法是使用探测将堆栈溢出移到明确定义的位置，而不是通过从任意堆栈溢出中进行回退。

## <a name="outofmemoryexception"></a>OutOfMemoryException

 ❌ 不要显式引发 <xref:System.OutOfMemoryException> 。 此异常仅由 CLR 基础结构引发。

## <a name="comexception-sehexception-and-executionengineexception"></a>ComException、SEHException 和 ExecutionEngineException

 ❌ 不要显式引发 <xref:System.Runtime.InteropServices.COMException> 、  <xref:System.ExecutionEngineException> 和 <xref:System.Runtime.InteropServices.SEHException> 。 这些异常仅由 CLR 基础结构引发。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [异常设计准则](exceptions.md)
