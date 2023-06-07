---
title: 复制和更新记录表达式
description: 了解如何编写复制现有记录或匿名记录、更新指定字段以及返回更新的记录或匿名记录的"复制和更新表达式"。
author: ChrSteinert
ms.date: 06/12/2019
ms.openlocfilehash: bec3e0ac2fb34e358b199c509c4599b55d7bf35e
ms.sourcegitcommit: 465547886a1224a5435c3ac349c805e39ce77706
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81739278"
---
# <a name="copy-and-update-record-expressions"></a>复制和更新记录表达式

*复制和更新记录表达式*是复制现有记录、更新指定字段并返回更新的记录的表达式。

## <a name="syntax"></a>语法

```fsharp
{ record-name with
    updated-labels }

{| anonymous-record-name with
    updated-labels |}
```

## <a name="remarks"></a>备注

默认情况下，记录和匿名记录是不可变的，因此无法更新现有记录。 要创建更新的记录，必须再次指定记录的所有字段。 为了简化此任务，可以使用*复制和更新表达式*。 此表达式采用现有记录，使用表达式中的指定字段和表达式指定的缺失字段创建新的相同类型。

当您必须复制现有记录并可能更改某些字段值时，这非常有用。

例如，以新创建的记录为例。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1905.fs)]

要仅更新该记录中的两个字段，可以使用*复制和更新记录表达式*：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1906.fs)]

## <a name="see-also"></a>另请参阅

- [记录](records.md)
- [匿名记录](anonymous-records.md)
- [F# 语言参考](index.md)
