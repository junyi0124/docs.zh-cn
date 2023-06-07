---
title: '可移植性和互操作性规则 (代码分析) '
description: 了解代码分析规则可移植性和互操作性规则
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.Portablityrules
- vs.codeanalysis.Interoperabilityrules
helpviewer_keywords:
- managed code analysis rules, interoperability rules, portability rules
- portability rules
- warnings, portability
- interoperability rules
- warnings, interoperability
author: gewarren
ms.author: gewarren
ms.openlocfilehash: a20cd77e13c4a8b95633d129990667f0a8de3ee8
ms.sourcegitcommit: 2e4adc490c1d2a705a0592b295d606b10b9f51f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "96590173"
---
# <a name="portability-and-interoperability-rules"></a>可移植性和互操作性规则

可移植性规则支持跨不同平台的可移植性。 互操作性规则支持与 COM 客户端的交互。

## <a name="in-this-section"></a>在本节中

| 规则 | 描述 |
| - | - |
| [CA1401： P/Invoke 应不可见](ca1401.md) | 公共类型中的公共或受保护方法具有 System.Runtime.InteropServices.DllImportAttribute 特性 (也由 Visual Basic) 中的 Declare 关键字实现。 这些方法不能公开。 |
| [CA1416：验证平台兼容性](ca1416.md) | 在组件上使用平台相关的 Api 使代码在所有平台上都不再工作。 |
| [CA1417：不 `OutAttribute` 在 P/invoke 的字符串参数上使用](ca1417.md) | 如果字符串是暂存的字符串，则通过值传递的字符串参数与 `OutAttribute` 运行时的不稳定性。 |
