---
title: Lambda 表达式:趣味关键字
description: 了解如何使用F# "趣味" 关键字定义 lambda 表达式, 这是匿名函数。
ms.date: 05/16/2016
ms.openlocfilehash: 9818724686dd83a7e352fb36819289fa19b002df
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630664"
---
# <a name="lambda-expressions-the-fun-keyword-f"></a>Lambda 表达式:趣味关键字 (F#)

`fun`关键字用于定义 lambda 表达式, 即匿名函数。

## <a name="syntax"></a>语法

```fsharp
fun parameter-list -> expression
```

## <a name="remarks"></a>备注

*参数列表*通常包含名称和参数类型 (可选)。 更常见的情况是,*参数列表*可以包含任何F#模式。 有关可能模式的完整列表, 请参阅[模式匹配](../pattern-matching.md)。 有效参数的列表包括以下示例。

```fsharp
// Lambda expressions with parameter lists.
fun a b c -> ...
fun (a: int) b c -> ...
fun (a : int) (b : string) (c:float) -> ...

// A lambda expression with a tuple pattern.
fun (a, b) -> …

// A lambda expression with a list pattern.
fun head :: tail -> …
```

*表达式*是函数的主体, 后者是生成返回值的的最后一个表达式。 有效的 lambda 表达式的示例包括:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet301.fs)]

## <a name="using-lambda-expressions"></a>使用 Lambda 表达式

如果要对列表或其他集合执行操作, 并且想要避免定义函数的额外工作, Lambda 表达式特别有用。 许多F#库函数将函数值作为参数使用, 在这种情况下, 使用 lambda 表达式会非常方便。 下面的代码将 lambda 表达式应用于列表的元素。 在这种情况下, 匿名函数向列表中的每个元素加1。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet302.fs)]

## <a name="see-also"></a>请参阅

- [函数](index.md)
