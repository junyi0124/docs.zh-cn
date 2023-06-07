---
title: 错误的校验和值、非十六进制数字或奇数个十六进制数字
ms.date: 07/20/2015
f1_keywords:
- bc42033
- vbc42033
helpviewer_keywords:
- BC42033
ms.assetid: 4575554d-3615-46e4-9c6a-18e9c338e4ed
ms.openlocfilehash: e380033f9353781ee7dc86696e93c8d0f18a1a73
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162968"
---
# <a name="bc42033-bad-checksum-value-non-hex-digits-or-odd-number-of-hex-digits"></a>BC42033：错误的校验和值、非十六进制数字或奇数个十六进制数字

校验和值包含无效的十六进制数字，或数字个数为奇数。

 当 ASP.NET 生成 Visual Basic 源文件（扩展名为 .vb）时，它将计算校验和，并将其放在一个由 `#externalchecksum` 标识的隐藏的源文件中。 生成 .vb 文件的用户也可以执行此操作，但最好将此过程保留为内部使用。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC42033

## <a name="to-correct-this-error"></a>更正此错误

1. 如果 ASP.NET 正在生成 Visual Basic 源文件，请重新开始项目生成。

2. 如果重新开始后此警告仍然存在，请重新安装 ASP.NET 并重试生成。

3. 如果警告仍然存在或者你没有使用 ASP.NET，请收集有关此情形的信息，并通知 Microsoft 产品支持服务。

## <a name="see-also"></a>另请参阅

- [ASP.NET 概述](/aspnet/overview)
- [与我们交流](/visualstudio/ide/feedback-options)
