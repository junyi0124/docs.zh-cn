---
title: 循环：for...in 表达式
description: F#请参阅in 表达式循环构造用于循环访问可枚举集合中的模式匹配项。
ms.date: 05/16/2016
ms.openlocfilehash: 5a2ca59ca4199ece5d78010ff780e86ae2b25181
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216448"
---
# <a name="loops-forin-expression"></a>循环：for...in 表达式

此循环构造用于循环访问可枚举集合中的模式匹配，如范围表达式、序列、列表、数组或支持枚举的其他构造。

## <a name="syntax"></a>语法

```fsharp
for pattern in enumerable-expression do
    body-expression
```

## <a name="remarks"></a>备注

表达式可以与其他 .net 语言中`for each`的语句进行比较，因为它用于循环对可枚举集合中的值。 `for...in` 但是， `for...in`还支持对集合进行模式匹配，而不是只是对整个集合进行迭代。

可枚举的表达式可以指定为可枚举的集合，也可以通过`..`使用运算符作为整数类型的范围。 可枚举集合包括列表、序列、数组、集、映射等。 可以使用任何实现`System.Collections.IEnumerable`的类型。

使用`..`运算符表示范围时，可以使用以下语法。

*start* 。 *finish*

你还可以使用包含名为*skip*的增量的版本，如下面的代码所示。

*start* 。 *skip* 。 *finish*

如果使用整数范围和简单计数器变量作为模式，则典型的行为是在每次迭代时将计数器变量递增1，但如果该范围包含 skip 值，则该计数器将改为按 skip 值递增。

在模式中匹配的值还可以在正文表达式中使用。

下面的代码示例阐释了`for...in`表达式的用法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5201.fs)]

输出如下所示。

```console
1
5
100
450
788
```

下面的示例演示如何在序列上循环，以及如何使用元组模式而不是简单变量。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5202.fs)]

输出如下所示。

```console
1 squared is 1
2 squared is 4
3 squared is 9
4 squared is 16
5 squared is 25
6 squared is 36
7 squared is 49
8 squared is 64
9 squared is 81
10 squared is 100
```

下面的示例演示如何在一个简单的整数范围内循环。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5203.fs)]

Function1 的输出如下所示。

```console
1 2 3 4 5 6 7 8 9 10
```

下面的示例演示如何在 skip 为2的范围内循环，其中包括范围中的所有其他元素。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5204.fs)]

的输出`function2`如下所示。

```console
1 3 5 7 9
```

下面的示例演示如何使用字符范围。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5205.fs)]

的输出`function3`如下所示。

```console
a b c d e f g h i j k l m n o p q r s t u v w x y z
```

下面的示例演示如何对反向迭代使用负值 skip 值。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5208.fs)]

的输出`function4`如下所示。

```console
10 9 8 7 6 5 4 3 2 1 ... Lift off!
```

范围的开始和结束也可以是表达式，如函数，如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5206.fs)]

`function5`带有此输入的的输出如下所示。

```console
2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18
```

下一个示例演示在循环中不需要元素时\_，如何使用通配符（）。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5207.fs)]

输出如下所示。

```console
Number of elements in list1: 5
```

`Note`可以在序列`for...in`表达式和其他计算表达式中使用，在这种情况下，将使用`for...in`自定义版本的表达式。 有关详细信息，请参阅[序列](sequences.md)、[异步工作流](asynchronous-workflows.md)和[计算表达式](computation-expressions.md)。

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
- [循环`for...to`表达式](loops-for-to-expression.md)
- [循环`while...do`表达式](loops-while-do-expression.md)
