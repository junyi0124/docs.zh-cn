---
title: CoreResponseData 字段 m_ResponseHeaders
description: 了解 .NET 中的 m_ResponseHeaders CoreResponseData 字段。 此字段是包含与服务器响应关联的标头的设置了 webheadercollection 类型。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_ResponseHeaders
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 7c7b896193cb81e9fc9e3ec28110359003a36728
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989796"
---
# <a name="coreresponsedatam_responseheaders-field"></a>CoreResponseData \_ ResponseHeaders 字段

`CoreResponseData.m_ResponseHeaders`<xref:System.Net.WebHeaderCollection>与服务器响应关联的标头的。

## <a name="syntax"></a>语法
  
```csharp
public WebHeaderCollection m_ResponseHeaders
```

> [!WARNING]
> 此 API 不应在代码中直接使用。 相反，应使用 <xref:System.Diagnostics.DiagnosticSource> 挂钩网络代码。 请参阅[DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）

**.NET Framework 版本：** 自2.0 起可用。
