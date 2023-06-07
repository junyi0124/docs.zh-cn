---
title: 异常：try...with 表达式
description: 了解如何使用F# "try .。。带有用于异常处理的 "表达式"。
ms.date: 05/16/2016
ms.openlocfilehash: e4d615086a93e26b76cca460e797659ca13792b5
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630247"
---
# <a name="exceptions-the-trywith-expression"></a>异常：try...with 表达式

本主题介绍`try...with`表达式, 该表达式用于F#语言中的异常处理。

## <a name="syntax"></a>语法

```fsharp
try
    expression1
with
| pattern1 -> expression2
| pattern2 -> expression3
...
```

## <a name="remarks"></a>备注

表达式用于处理中F#的异常。 `try...with` 它与中`try...catch` C#的语句类似。 在上述语法中,*表达式*a 中的代码可能会生成异常。 表达式`try...with`将返回一个值。 如果没有引发异常, 则整个表达式将返回*表达式表达式*的值。 如果引发了异常, 则每个*模式*将依次与异常进行比较, 对于第一个匹配模式, 将执行对应的*表达式*(称为 "*异常处理程序*"), 并在整个表达式中返回该异常处理程序中的表达式的值。 如果没有模式匹配, 则异常将在调用堆栈中向上传播, 直到找到匹配的处理程序。 从异常处理程序中的每个表达式返回的值的类型必须与从`try`块中的表达式返回的类型匹配。

通常, 发生错误的一种情况是, 不存在可以从每个异常处理程序中的表达式返回的有效值。 常见的模式是将表达式的类型作为选项类型。 下面的代码示例演示了此模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5601.fs)]

异常可能是 .NET 异常, 也可能是F#异常。 您可以使用F# `exception`关键字来定义异常。

您可以使用多种模式筛选异常类型和其他条件;下表汇总了这些选项。

|模式|描述|
|-------|-----------|
|:? *exception-type*|匹配指定的 .NET 异常类型。|
|:? *异常类型*作为*标识符*|与指定的 .NET 异常类型匹配, 但为异常提供一个命名值。|
|*exception-name*(*arguments*)|匹配F#异常类型并绑定自变量。|
|*identifier*|匹配任何异常并将名称绑定到 exception 对象。 等效于 **:？Exception 作为**_标识符_|
|*条件*时*标识符*|如果条件为 true, 则匹配任何异常。|

## <a name="examples"></a>示例

下面的代码示例演示如何使用各种异常处理程序模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5602.fs)]

> [!NOTE]
> 构造是一个独立`try...finally`于表达式的表达式。 `try...with` 因此, 如果代码需要`with`块`finally`和块, 则必须嵌套两个表达式。

> [!NOTE]
> 可以在异步`try...with`工作流和其他计算表达式中使用, 在这种情况下, 将`try...with`使用自定义版本的表达式。 有关详细信息, 请参阅[异步工作流](../asynchronous-workflows.md)和[计算表达式](../computation-expressions.md)。

## <a name="see-also"></a>请参阅

- [异常处理](index.md)
- [异常类型](exception-types.md)
- [异常：`try...finally`表达式](the-try-finally-expression.md)
