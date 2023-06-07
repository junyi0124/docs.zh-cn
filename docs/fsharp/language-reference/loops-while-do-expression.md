---
title: 循环：while...do 表达式
description: 了解如何操作 .。。当指定的测试条件为 true 时，do expression 用于执行迭代执行（循环）。
ms.date: 05/16/2016
ms.openlocfilehash: 73526279358db101f8d07721a200920f1e87f119
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216633"
---
# <a name="loops-whiledo-expression"></a>循环：while...do 表达式

`while...do`表达式用于在指定的测试条件为 true 时执行迭代执行（循环）。

## <a name="syntax"></a>语法

```fsharp
while test-expression do
    body-expression
```

## <a name="remarks"></a>备注

计算*测试表达式*;如果为`true`，则执行*主体表达式*，并再次计算测试表达式。 *主体表达式*的类型`unit`必须为。 如果测试表达式为`false`，则迭代结束。

下面的示例阐释了`while...do`表达式的用法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5301.fs)]

上述代码的输出是介于1和20之间的随机数的流，其中最后一个值为10。

```console
13 19 8 18 16 2 10
Found a 10!
```

> [!NOTE]
> 可以在序列`while...do`表达式和其他计算表达式中使用，在这种情况下，将使用`while...do`自定义版本的表达式。 有关详细信息，请参阅[序列](sequences.md)、[异步工作流](asynchronous-workflows.md)和[计算表达式](computation-expressions.md)。

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
- [循环`for...in`表达式](loops-for-in-expression.md)
- [循环`for...to`表达式](loops-for-to-expression.md)
