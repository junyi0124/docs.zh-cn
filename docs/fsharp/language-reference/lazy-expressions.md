---
title: 延迟表达式
description: '了解 F # 惰性表达式如何提高应用程序和库的性能。'
ms.date: 08/15/2020
ms.openlocfilehash: 71c466ca3b74c9e92b81a3c268e07438ec944905
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558083"
---
# <a name="lazy-expressions"></a>延迟表达式

*惰性表达式* 是不会立即计算的表达式，而是在需要结果时计算的表达式。 这有助于提高代码的性能。

## <a name="syntax"></a>语法

```fsharp
let identifier = lazy ( expression )
```

## <a name="remarks"></a>备注

在前面的语法中， *expression* 是只在需要结果时计算的代码，而 *标识符* 是存储结果的值。 值的类型为 [`Lazy<'T>`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-lazy-1-0.html) ，其中用于的实际类型 `'T` 由表达式的结果确定。

利用迟缓表达式，你可以通过将表达式的执行限制为仅需要结果的情况来提高性能。

若要强制执行表达式，请调用方法 `Force` 。 `Force` 导致执行只执行一次。 后续调用将 `Force` 返回相同的结果，但不执行任何代码。

下面的代码演示了如何使用延迟表达式以及如何使用 `Force` 。 在此代码中，的类型 `result` 为 `Lazy<int>` ，并且 `Force` 方法返回 `int` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet73011.fs)]

迟缓计算（但不 `Lazy` 是类型）也用于序列。 有关详细信息，请参阅 [序列](sequences.md)。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [LazyExtensions 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-lazyextensions.html)
