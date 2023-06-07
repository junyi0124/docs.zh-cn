---
title: 源代码行标识符、文件标识符和路径标识符
description: 了解如何使用内置的F#标识符值，通过这些值可以访问代码中的源行号、目录和文件名。
ms.date: 05/16/2016
ms.openlocfilehash: f22c3dfb3cb106fbe45883ffd7de01feac30db00
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216749"
---
# <a name="source-line-file-and-path-identifiers"></a>源代码行标识符、文件标识符和路径标识符

标识符`__LINE__`， `__SOURCE_DIRECTORY__`和是内置值，使用这些值可以访问代码中的源行号、目录和文件名。`__SOURCE_FILE__`

## <a name="syntax"></a>语法

```fsharp
__LINE__
__SOURCE_DIRECTORY__
__SOURCE_FILE__
```

## <a name="remarks"></a>备注

其中每个值都有`string`类型。

下表汇总了中F#可用的源行、文件和路径标识符。 这些标识符不是预处理器宏;它们是编译器识别的内置值。

|预定义标识符|描述|
|---------------------|-----------|
|`__LINE__`|计算结果为当前行号，考虑`#line`指令。|
|`__SOURCE_DIRECTORY__`|计算结果为源目录的当前完整路径，考虑`#line`指令。|
|`__SOURCE_FILE__`|计算结果为当前源文件名，而不包含其路径， `#line`考虑指令。|

有关`#line`指令的详细信息，请参阅[编译器指令](compiler-directives.md)。

## <a name="example"></a>示例

下面的代码示例演示如何使用这些值。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7401.fs)]

输出：

```console
Line: 4
Source Directory: C:\Users\username\Documents\Visual Studio 2017\Projects\SourceInfo\SourceInfo
Source File: Program.fs
```

## <a name="see-also"></a>请参阅

- [编译器指令](compiler-directives.md)
- [F# 语言参考](index.md)
