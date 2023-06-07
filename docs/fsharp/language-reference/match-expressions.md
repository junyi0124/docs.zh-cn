---
title: 匹配表达式
description: 了解F# match 表达式如何提供基于表达式与一组模式比较的分支控件。
ms.date: 04/19/2018
ms.openlocfilehash: 222cb0604300039d86ed0c80293651631d212eb6
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68627606"
---
# <a name="match-expressions"></a>匹配表达式

`match`表达式提供基于表达式与一组模式比较的分支控件。

## <a name="syntax"></a>语法

```fsharp
// Match expression.
match test-expression with
| pattern1 [ when condition ] -> result-expression1
| pattern2 [ when condition ] -> result-expression2
| ...

// Pattern matching function.
function
| pattern1 [ when condition ] -> result-expression1
| pattern2 [ when condition ] -> result-expression2
| ...
```

## <a name="remarks"></a>备注

模式匹配表达式允许基于测试表达式与一组模式的比较来实现复杂的分支。 在表达式中,*测试表达式*依次与每个模式进行比较, 当找到匹配项时, 将计算相应的*结果表达式*, 并将结果值作为匹配表达式的值返回。 `match`

前面的语法中所示的模式匹配函数是一个 lambda 表达式, 该表达式中的模式匹配会立即在参数上执行。 上面的语法中所示的模式匹配函数等效于以下形式。

```fsharp
fun arg ->
    match arg with
    | pattern1 [ when condition ] -> result-expression1
    | pattern2 [ when condition ] -> result-expression2
    | ...
```

有关 lambda 表达式的详细信息, 请[参阅 lambda 表达式:`fun`关键字。](./functions/lambda-expressions-the-fun-keyword.md)

整个模式集应涵盖输入变量的所有可能的匹配项。 通常, 使用通配符模式 (`_`) 作为最后一种模式, 以匹配任何以前未匹配的输入值。

下面的代码演示了使用`match`表达式的部分方法。 有关可以使用的所有可能模式的参考和示例, 请参阅[模式匹配](pattern-matching.md)。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4601.fs)]

## <a name="guards-on-patterns"></a>模式保护

您可以使用`when`子句来指定变量为匹配模式而必须满足的附加条件。 此类子句称为*临界*。 除非对与该`when`保护关联的模式进行匹配, 否则不会计算关键字后面的表达式。

下面的示例说明了如何使用临界指定变量模式的数值范围。 请注意, 使用布尔运算符组合多个条件。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4602.fs)]

请注意, 由于不能在模式中使用文本以外的值, 因此, 如果`when`必须将部分输入与值进行比较, 则必须使用子句。 下面的代码对此进行了演示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4603.fs)]

请注意, 当联合模式被临界覆盖时, 此防护将应用于**所有**模式, 而不仅仅是最后一个。 例如, 在以下代码中, guard `when a > 12`适用`A a`于和`B a`:

```fsharp
type Union =
    | A of int
    | B of int

let foo() =
    let test = A 42
    match test with
    | A a
    | B a when a > 41 -> a // the guard applies to both patterns
    | _ -> 1

foo() // returns 42
```

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
- [活动模式](active-patterns.md)
- [模式匹配](pattern-matching.md)
