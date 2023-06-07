---
title: do 绑定
description: 了解如何使用F# "do" 绑定来执行代码, 而无需定义函数或值。
ms.date: 05/16/2016
ms.openlocfilehash: f98f523296bfaceeda35d4861eafbfeaa5a60c32
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630540"
---
# <a name="do-bindings"></a>do 绑定

`do`绑定用于在不定义函数或值的情况下执行代码。 此外, do 绑定可以在类中使用, 请参阅[ `do`类中的绑定](../members/do-bindings-in-classes.md)。

## <a name="syntax"></a>语法

```fsharp
[ attributes ]
[ do ]expression
```

## <a name="remarks"></a>备注

当你想要独立于函数或值定义执行代码时, 请使用绑定。`do` `do`绑定中的表达式必须返回`unit`。 初始化模块时, 将执行`do`顶级绑定中的代码。 关键字`do`是可选的。

特性可应用于顶级`do`绑定。 例如, 如果您的程序使用 COM 互操作, 则您可能需要将`STAThread`该属性应用于程序。 为此, 可以使用`do`绑定上的属性, 如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet201.fs)]

## <a name="see-also"></a>请参阅

- [F# 语言参考](../index.md)
- [函数](index.md)
