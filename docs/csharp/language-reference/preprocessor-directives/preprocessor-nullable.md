---
description: '#可为空 - C# 参考'
title: '#可为空 - C# 参考'
ms.date: 11/18/2020
f1_keywords:
- '#nullable'
helpviewer_keywords:
- '#nullable directive [C#]'
ms.openlocfilehash: 4c114a09f29769fcc824bcdabdc1d523e33f199d
ms.sourcegitcommit: 30e9e11dfd90112b8eec6406186ba3533f21eba1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2020
ms.locfileid: "95099444"
---
# <a name="nullable-c-reference"></a>#nullable（C# 参考）

`#nullable` 预处理器指令将设置可为空注释上下文和可为空警告上下文 。 此指令控制是否可为空注释是否有效，以及是否给出为 Null 性警告。 每个上下文要么处于已禁用状态，要么处于已启用状态 。

可在项目级别（C# 源代码之外）指定这两个上下文。 `#nullable` 指令控制注释和警告上下文，并优先于项目级设置。 指令会设置其控制的上下文，直到另一个指令替代它，或直到源文件结束为止。

指令的效果如下所示：

- `#nullable disable`：将可为空注释和警告上下文设置为“已禁用”。
- `#nullable enable`：将可为空注释和警告上下文设置为“已启用”。
- `#nullable restore`：将可为空注释和警告上下文还原为项目设置。
- `#nullable disable annotations`：将可为空注释上下文设置为“已禁用”。
- `#nullable enable annotations`：将可为空注释上下文设置为“已启用”。
- `#nullable restore annotations`：将可为空注释上下文还原为项目设置。
- `#nullable disable warnings`：将可为空警告上下文设置为“已禁用”。
- `#nullable enable warnings`：将可为空警告上下文设置为“已启用”。
- `#nullable restore warnings`：将可为空警告上下文还原为项目设置。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 预处理器指令](./index.md)
