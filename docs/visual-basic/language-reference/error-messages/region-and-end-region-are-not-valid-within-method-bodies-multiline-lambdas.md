---
title: “#Region”和“#End Region”语句在方法体/多行 lambda 内无效
ms.date: 07/20/2015
f1_keywords:
- bc32025
- vbc32025
helpviewer_keywords:
- BC32025
ms.assetid: 43707bf1-1c6b-4d82-b081-e5a17dca51c1
ms.openlocfilehash: c792fa1a42bde22959ae21439cb2a0a1e4be343f
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162279"
---
# <a name="bc32025-region-and-end-region-statements-are-not-valid-within-method-bodiesmultiline-lambdas"></a>BC32025： "#Region" 和 "#End Region" 语句在方法体/多行 lambda 内无效

`#Region`必须在类、模块或命名空间级别声明块。 可折叠区域可以包含一个或多个过程，但不能在过程中开始或结束。

 **错误 ID：** BC32025

## <a name="to-correct-this-error"></a>更正此错误

1. 确保使用或语句正确终止前面的过程 `End Function` `End Sub` 。

2. 确保 `#Region` 和 `#End Region` 指令位于同一个代码块中。

## <a name="see-also"></a>另请参阅

- [#Region 指令](../directives/region-directive.md)
