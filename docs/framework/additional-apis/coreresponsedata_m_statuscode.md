---
title: CoreResponseData 字段 m_StatusCode
description: 阅读有关 .NET 中的 m_StatusCode CoreResponseData 字段的信息。 此字段是包含 HTTP 响应状态的 HttpStatusCode 类型。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_StatusCode
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 05950290bde96511432941ce679e663126878663
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989774"
---
# <a name="coreresponsedatam_statuscode-field"></a>CoreResponseData \_ StatusCode 字段

`CoreResponseData.m_StatusCode`<xref:System.Net.HttpStatusCode>包含响应状态的。

## <a name="syntax"></a>语法
  
```csharp
public HttpStatusCode m_StatusCode
```

> [!WARNING]
> 此 API 不应在代码中直接使用。 相反，应使用 <xref:System.Diagnostics.DiagnosticSource> 挂钩网络代码。 请参阅[DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）

**.NET Framework 版本：** 自2.0 起可用。
