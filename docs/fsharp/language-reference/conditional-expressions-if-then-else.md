---
title: 条件表达式： if .。。then .。。其它
description: 了解如何编写中F#的条件表达式来执行不同的代码分支。
ms.date: 05/16/2016
ms.openlocfilehash: d9763f37cd9170c42e968282b54a3ce925e92591
ms.sourcegitcommit: a2d0e1f66367367065bc8dc0dde488ab536da73f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2019
ms.locfileid: "71083024"
---
# <a name="conditional-expressions-ifthenelse"></a>条件表达式：`if...then...else`

`if...then...else`表达式运行不同的代码分支，还根据给定的布尔表达式计算出不同的值。

## <a name="syntax"></a>语法

```fsharp
if boolean-expression then expression1 [ else expression2 ]
```

## <a name="remarks"></a>备注

在前面的语法中，如果布尔表达式的计算结果为`true`，则*表达式*1 将运行;*否则，表达式*2 将运行。

与其他语言不同， `if...then...else`构造是一个表达式，而不是语句。 这意味着它会生成一个值，该值是执行的分支中最后一个表达式的值。 每个分支中生成的值的类型必须匹配。 如果没有显式`else`分支，则其类型为`unit`。 因此，如果`then`分支的类型是以外的任何类型`unit` `else` ，则必须具有具有相同返回类型的分支。 将表达式`if...then...else`链接在一起时，可以使用关键字`elif`而不`else if`是; 它们是等效的。

## <a name="example"></a>示例

下面的示例演示如何使用`if...then...else`表达式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4501.fs)]

```console
10 is less than 20
What is your name? John
How old are you? 9
You are only 9 years old and already learning F#? Wow!
```

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
