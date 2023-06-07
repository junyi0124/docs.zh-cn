---
ms.openlocfilehash: 394f2daebad7b6af94bee4d7876796e87fe8ab19
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031970"
---
### <a name="handling-corrupted-state-exceptions-is-not-supported"></a>不支持处理损坏状态异常

不支持在 .NET Core 中处理损坏进程状态异常。

#### <a name="change-description"></a>更改描述

以前，损坏进程状态异常可以由托管代码异常处理程序进行捕获和处理，例如在 C# 中使用 [try-catch](../../../../docs/csharp/language-reference/keywords/try-catch.md) 语句。

从 .NET Core 1.0 开始，损坏进程状态异常无法由托管代码进行处理。 公共语言运行时不会将损坏进程状态异常传递给托管代码。

#### <a name="version-introduced"></a>引入的版本

1.0

#### <a name="recommended-action"></a>建议操作

通过解决导致这些异常的情况来避免需要处理损坏进程状态异常。 如果绝对有必要处理损坏进程状态异常，请在 C 或 C++ 代码中编写异常处理程序。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute?displayProperty=fullName>
- [legacyCorruptedStateExceptionsPolicy 元素](~/docs/framework/configure-apps/file-schema/runtime/legacycorruptedstateexceptionspolicy-element.md)

<!--

#### Affected APIs

- `T:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute`

-->
