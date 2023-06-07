---
title: 异常：failwith 函数
description: 了解 "failwith" 函数如何生成F#异常。
ms.date: 05/16/2016
ms.openlocfilehash: f2c86362d5bdde7bab55751f019965a5f4ca6236
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630323"
---
# <a name="exceptions-the-failwith-function"></a>异常：failwith 函数

函数会生成F# `failwith`

## <a name="syntax"></a>语法

```fsharp
failwith error-message-string
```

## <a name="remarks"></a>备注

前面语法中的*错误消息字符串*是文本字符串或类型`string`的值。 它将成为`Message`异常的属性。

生成`failwith`的异常是一个`System.Exception`异常, 该异常是在代码中`Failure` F#具有名称的引用。 下面的代码演示`failwith`如何使用来引发异常。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6001.fs)]

## <a name="see-also"></a>请参阅

- [异常处理](index.md)
- [异常类型](exception-types.md)
- [异常：`try...with`表达式](the-try-with-expression.md)
- [异常：`try...finally`表达式](the-try-finally-expression.md)
- [异常：`raise` 函数](the-raise-function.md)
