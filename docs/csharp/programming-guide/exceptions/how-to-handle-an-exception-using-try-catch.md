---
title: 如何使用 try-catch 处理异常 - C# 编程指南
description: 了解如何使用 try-catch 块处理异常。 查看代码示例和其他可用资源。
ms.date: 12/09/2020
helpviewer_keywords:
- exception handling [C#], try/catch blocks
- exceptions [C#], try/catch blocks
- try/catch blocks [C#]
ms.assetid: ca8e3773-980e-4767-8633-7408540e9818
ms.openlocfilehash: b6368660dbe037123f5bb6ce52502d4a94fcfc3a
ms.sourcegitcommit: 9b877e160c326577e8aa5ead22a937110d80fa44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97110515"
---
# <a name="how-to-handle-an-exception-using-trycatch-c-programming-guide"></a>如何使用 try/catch 处理异常（C# 编程指南）

[try-catch](../../language-reference/keywords/try-catch.md) 块的用途是捕获并处理工作代码产生的异常。 某些异常可以在 `catch` 块中进行处理，问题得以解决并不再出现异常；但是，大多数情况下你唯一可做的是确保引发的异常是合理异常。

## <a name="example"></a>示例

在此示例中，<xref:System.IndexOutOfRangeException> 不是最合理的异常：<xref:System.ArgumentOutOfRangeException> 对于此方法来说更有意义，因为此错误是由调用方传递的 `index` 参数引起的。

:::code language="csharp" source="snippets/exceptions/ExampleTryCatch.cs" id="ExampleTryCatch":::

## <a name="comments"></a>注释

引发异常的代码包含在 `try` 块中。 在此块后面紧挨着添加 `catch` 语句以处理 `IndexOutOfRangeException` 异常（如果发生此异常）。 `catch` 块处理 `IndexOutOfRangeException` 异常并改为引发更合理的 `ArgumentOutOfRangeException` 异常。 为了向调用方提供尽可能多的信息，请考虑将原始异常指定为新异常的 <xref:System.Exception.InnerException%2A>。 因为 <xref:System.Exception.InnerException%2A> 属性为 [read-only](../../properties.md#read-only)，所以必须在新异常的构造函数中指定此属性。
