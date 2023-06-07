---
title: TlsStream. m_Worker 字段（System.Net）
ms.date: 10/21/2019
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.TlsStream.m_Worker
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: d335820d13e1e15e054e824a284615cdbf6c2094
ms.sourcegitcommit: 337bdc5a463875daf2cc6883e5a2da97d56f5000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72847240"
---
# <a name="tlsstreamm_worker-field"></a>TlsStream. m_Worker 字段

表示 SSL 流的状态。

## <a name="syntax"></a>语法

```csharp
private SslState m_Worker;
```

## <a name="field-value"></a>字段值

`System.Net.Security.SslState`  
SSL 流的状态。

## <a name="remarks"></a>备注

> [!WARNING]
> `TlsStream.m_Worker` 字段是专用的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此字段。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（在 System.web 中）

**.NET Framework 版本：** 自2.0 起可用。
