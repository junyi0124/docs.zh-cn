---
title: 断言
description: 了解如何使用 "assert" 表达式作为F#编程语言中的测试表达式的调试功能。
ms.date: 10/22/2019
ms.openlocfilehash: 430db20e5ca307ba43a72e678a0424e03b0ac381
ms.sourcegitcommit: 9bd1c09128e012b6e34bdcbdf3576379f58f3137
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2019
ms.locfileid: "72799061"
---
# <a name="assertions"></a>断言

`assert` 表达式是一种可用于测试表达式的调试功能。 在调试模式中遇到故障时，断言将生成一个系统错误对话框。

## <a name="syntax"></a>语法

```fsharp
assert condition
```

## <a name="remarks"></a>备注

`assert` 表达式的类型为 `bool -> unit`。

`assert` 函数可解析为 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>。 这意味着，其行为等同于直接调用 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>。

仅当在调试模式下进行编译时，才启用断言检查;也就是说，如果定义了常量 `DEBUG`。 在项目系统中，默认情况下，`DEBUG` 常数是在调试配置中定义的，而不是在发布配置中定义的。

使用F#异常处理无法捕获断言失败错误。

## <a name="example"></a>示例

下面的代码示例演示了 `assert` 表达式的用法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5401.fs)]

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
