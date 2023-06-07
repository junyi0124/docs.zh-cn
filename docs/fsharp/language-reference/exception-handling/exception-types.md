---
title: 异常类型
description: '了解如何定义和使用 F # 异常类型。'
ms.date: 05/16/2016
ms.openlocfilehash: 8b4ceec31a2d68abbcd025812ffeeefc0c090efb
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557224"
---
# <a name="exception-types"></a>异常类型

F #： .NET 异常类型和 F # 异常类型中有两种类别的异常。 本主题介绍如何定义和使用 F # 异常类型。

## <a name="syntax"></a>语法

```fsharp
exception exception-type of argument-type
```

## <a name="remarks"></a>备注

在前面的语法中， *exception 类型* 是新 F # 异常类型的名称，而 *参数类型* 表示当你引发此类型的异常时可以提供的参数的类型。 您可以通过使用 *参数类型*的元组类型指定多个参数。

F # 异常的典型定义如下所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5501.fs)]

您可以使用函数生成此类型的异常 `raise` ，如下所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5502.fs)]

您可以直接在表达式的筛选器中使用 F # 异常类型 `try...with` ，如下面的示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5503.fs)]

使用 F # 中的关键字定义的异常类型 `exception` 是从继承的新类型 `System.Exception` 。

## <a name="see-also"></a>请参阅

- [异常处理](index.md)
- [异常：`raise` 函数](the-raise-function.md)
- [异常层次结构](../../../standard/exceptions/index.md)
