---
title: 入口点
description: 了解如何将入口点设置为编译为F#可执行文件的程序, 在该文件中, 执行正式启动。
ms.date: 05/16/2016
ms.openlocfilehash: 5e13416131d4dfd22583439fedf51f18f7a461da
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630518"
---
# <a name="entry-point"></a>入口点

本主题介绍用于将入口点设置为F#程序的方法。

## <a name="syntax"></a>语法

```fsharp
[<EntryPoint>]
let-function-binding
```

## <a name="remarks"></a>备注

在前面的语法中,*函数绑定*是`let`绑定中函数的定义。

编译为可执行文件的程序的入口点是执行正式启动的位置。 通过将`EntryPoint`特性应用于程序的F# `main`函数, 可以指定应用程序的入口点。 此函数 (使用`let`绑定创建) 必须是最后编译的文件中的最后一个函数。 最后编译的文件是项目中的最后一个文件, 或者是传递到命令行的最后一个文件。

入口点函数的类型`string array -> int`为。 在命令行上提供的参数会传递到字符串`main`数组中的函数。 数组的第一个元素是第一个参数;数组中未包含可执行文件的名称, 因为它采用其他一些语言。 返回值用作进程的退出代码。 零通常指示成功;非零值表示错误。 非零返回代码的特定含义没有约定;返回代码的含义是特定于应用程序的。

下面的示例演示一个简单`main`的函数。

[!code-fsharp[Main](~/samples/snippets/fsharp/entry-point/snippet501.fs)]

当通过命令行`EntryPoint.exe 1 2 3`执行此代码时, 输出如下所示。

```console
Arguments passed to function : [|"1"; "2"; "3"|]
```

## <a name="implicit-entry-point"></a>隐式入口点

当程序没有显式指示入口点的**EntryPoint**属性时, 最后一个要编译的文件中的顶级绑定将用作入口点。

## <a name="see-also"></a>请参阅

- [函数](index.md)
- [let 绑定](let-bindings.md)
