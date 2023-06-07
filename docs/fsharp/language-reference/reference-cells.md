---
title: 引用单元格
description: 了解如何F#使用引用单元格来创建具有引用语义的可变值的存储位置。
ms.date: 05/16/2016
ms.openlocfilehash: 2bca7797b272c0e7d5bf54df07041dc08e33709a
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216781"
---
# <a name="reference-cells"></a>引用单元格

*引用单元*是使你能够创建具有引用语义的可变值的存储位置。

## <a name="syntax"></a>语法

```fsharp
ref expression
```

## <a name="remarks"></a>备注

可以在值前面使用 `ref` 运算符来创建用于封装该值的新引用单元格。 然后，您可以更改基础值，因为该值是可变的。

引用单元格容纳实际值；它不仅仅是一个地址。 使用 `ref` 运算符创建引用单元格时，将以封装的可变值的形式创建基础值的副本。

可以使用 `!`（感叹号）运算符取消对引用单元格的引用。

下面的代码示例阐释了引用单元格的声明和用法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2201.fs)]

输出为 `50`。

引用单元格是 `Ref` 泛型记录类型的实例，声明方式如下所示。

```fsharp
type Ref<'a> =
{ mutable contents: 'a }
```

类型 `'a ref` 是 `Ref<'a>` 的同义词。 IDE 中的编译器和 IntelliSense 显示此类型的前者，而基础定义是后者。

`ref` 运算符创建新引用单元格。 下面的代码声明 `ref` 运算符。

```fsharp
let ref x = { contents = x }
```

下表显示了可用于引用单元格的功能。

|运算符、成员或字段|描述|类型|定义|
|--------------------------|-----------|----|----------|
|`!`（取消引用运算符）|返回基础值。|`'a ref -> 'a`|`let (!) r = r.contents`|
|`:=`（赋值运算符）|更改基础值。|`'a ref -> 'a -> unit`|`let (:=) r x = r.contents <- x`|
|`ref`（运算符）|将值封装到新的引用单元格中。|`'a -> 'a ref`|`let ref x = { contents = x }`|
|`Value`（属性）|获取或设置基础值。|`unit -> 'a`|`member x.Value = x.contents`|
|`contents`（记录字段）|获取或设置基础值。|`'a`|`let ref x = { contents = x }`|

可通过多种方式来访问基础值。 取消引用运算符 (`!`) 返回的值不是可赋值的值。 因此，如果要修改基础值，您必须改用赋值运算符 (`:=`)。

`Value` 属性和 `contents` 字段都是可赋值的值。 因此，您可以使用它们来访问或更改基础值，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2203.fs)]

输出如下所示。

```console
10
10
11
12
```

提供字段 `contents` 的目的是为了与其他版本的 ML 兼容，并且该字段将在编译过程中产生警告。 若要禁用警告，请使用 `--mlcompatibility` 编译器选项。 有关详细信息，请参阅[编译器选项](compiler-options.md)。

C#程序员`ref`应知道在中C# ，与`ref`中F#的不同。 中F#的等效构造是[byref](byrefs.md)，这是不同于引用单元的概念。

如果由闭`mutable`包捕获，则标记`'a ref`为的值可能会自动提升为; 请参阅[值](./values/index.md)。

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
- [参数和自变量](parameters-and-arguments.md)
- [符号和运算符参考](./symbol-and-operator-reference/index.md)
- [值](./values/index.md)
