---
title: s_isDebuggerCheckDisabledForTestPurposes 字段
ms.date: 03/30/2017
ms.technology: dotnet-wpf
topic_type:
- apiref
api_name:
- System.Windows.Diagnostics.VisualDiagnostics.s_isDebuggerCheckDisabledForTestPurposes
api_location:
- PresentationCore.dll
api_type:
- Assembly
ms.assetid: 9033a513-c255-4f31-b6d7-09b8d8c50e2d
ms.openlocfilehash: ad9bc0ecf4b7a8e5f3ef13fdff5aa59ca8915922
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65634660"
---
# <a name="sisdebuggercheckdisabledfortestpurposes-field"></a>s_isDebuggerCheckDisabledForTestPurposes 字段

在此专用字段`System.Windows.Diagnostics.VisualDiagnostics`类由 Visual Studio 用来确定是否将执行活动的调试器内部检查。

## <a name="syntax"></a>语法

```csharp
private static bool s_isDebuggerCheckDisabledForTestPurposes
```

> [!WARNING]
> 中的 Api`System.Windows.Diagnostics.VisualDiagnostics`当在调试器下运行应用程序时，类才可用。 设置`s_isDebuggerCheckDisabledForTestPurposes`到`true`访问调试器外的 Api。
>
> Microsoft 不支持在生产应用程序在任何情况下使用此字段。

## <a name="requirements"></a>要求

**Namespace**：<xref:System.Windows.Diagnostics>

**程序集：** PresentationCore （在 PresentationCore.dll 中)

**.NET framework 版本：** 自 4.6 之后可用。
