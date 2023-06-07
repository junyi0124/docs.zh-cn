---
title: 类型缩写
description: 若要F#使代码更易于阅读, 请了解类型缩写, 为类型提供更有意义的名称。
ms.date: 05/16/2016
ms.openlocfilehash: 339b22a675e3f1ad8a3da207053e611942b55a22
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630214"
---
# <a name="type-abbreviations"></a>类型缩写

*类型缩写*为类型的别名或替换名称。

## <a name="syntax"></a>语法

```fsharp
type [accessibility-modifier] type-abbreviation = type-name
```

## <a name="remarks"></a>备注

您可以使用类型缩写为类型提供更有意义的名称, 从而使代码更易于阅读。 你还可以使用它们为类型创建易于使用的名称, 此名称在编写时不太繁琐。此外, 还可以使用类型缩写来更轻松地更改基础类型, 而无需更改所有使用该类型的代码。 下面是一个简单的类型缩写。

缩写词类型默认值为`public`。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2301.fs)]

类型缩写词可以包含泛型参数, 如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2302.fs)]

在前面的代码中`Transform` , 是一种类型缩写, 它表示一个函数, 该函数采用任何类型的单个自变量, 并返回该类型的单个值。

不会在 .NET Framework MSIL 代码中保留类型缩写。 因此, 当您使用另F#一个 .NET Framework 语言的程序集时, 您必须使用类型缩写的基础类型名称。

类型缩写还可用于度量单位。 有关详细信息, 请参阅[度量单位](units-of-measure.md)。

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
