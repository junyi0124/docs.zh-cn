---
title: 数值和比较运算符
ms.date: 03/30/2017
ms.assetid: 25b4a26a-06f2-4f80-87a9-76705ed46197
ms.openlocfilehash: 7e7af725864aa191f092055fa32b403093321aa5
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70781294"
---
# <a name="numeric-and-comparison-operators"></a>数值和比较运算符

算术运算符和比较运算符在公共语言运行库 (CLR) 中按预期方式工作，但有以下几点例外：

- SQL 不支持对浮点数字使用取模运算符。

- SQL 不支持未校验的算法。

- 在无法复制到 SQL 中的表达式中使用增量和减量运算符时会产生副作用，因而不支持此类运算符。

## <a name="supported-operators"></a>支持的运算符

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持以下运算符。

- 基本算术运算符：

  - `+`

  - `-`（减号）

  - `*`

  - `/`

  - Visual Basic 整除 (`\`)

  - `%` (Visual Basic `Mod`)

  - `<<`

  - `>>`

  - `-`（一元求反）

- 基本比较运算符：

  - Visual Basic `=` 和 C# `==`

  - Visual Basic `<>` 和 C# `!=`

  - Visual Basic `Is/IsNot`

  - `<`

  - `<=`

  - `>`

  - `>=`

## <a name="see-also"></a>请参阅

- [数据类型和函数](data-types-and-functions.md)
- [C# 运算符](../../../../../csharp/language-reference/operators/index.md)
- [运算符](../../../../../visual-basic/language-reference/operators/index.md)
