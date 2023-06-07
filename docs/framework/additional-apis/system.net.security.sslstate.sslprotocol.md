---
title: SslState. SslProtocol 属性（系统 .Net）
ms.date: 10/21/2019
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Security.SslState.SslProtocol
- System.Net.Security.SslState.get_SslProtocol
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 6983ac071dad29b240308031ecd0a3562a6856e4
ms.sourcegitcommit: 337bdc5a463875daf2cc6883e5a2da97d56f5000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72847246"
---
# <a name="sslstatesslprotocol-property"></a>SslState. SslProtocol 属性

获取 SSL 协议版本。

## <a name="syntax"></a>语法

```csharp
internal SslProtocols SslProtocol { get; }
```

## <a name="property-value"></a>属性值

<xref:System.Security.Authentication.SslProtocols>  
枚举值的按位组合，这些枚举值指定 SSL 协议版本。

## <a name="remarks"></a>备注

> [!WARNING]
> `SslState.SslProtocol` 属性是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此属性。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net.Security>

**程序集：** 系统（在 System.web 中）

**.NET Framework 版本：** 自2.0 起可用。
