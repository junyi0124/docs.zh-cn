---
title: PooledStream. NetworkStream 属性（System.Net）
ms.date: 10/21/2019
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.PooledStream.NetworkStream
- System.Net.PooledStream.get_NetworkStream
- System.Net.PooledStream.set_NetworkStream
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 541b8c94b30675c1286b48a2291c3bd3e4aeea0b
ms.sourcegitcommit: 337bdc5a463875daf2cc6883e5a2da97d56f5000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72847228"
---
# <a name="pooledstreamnetworkstream-property"></a>PooledStream. NetworkStream 属性

获取或设置 `PooledStream` 套接字的网络流。

## <a name="syntax"></a>语法

```csharp
internal NetworkStream NetworkStream { get; set; }
```

## <a name="property-value"></a>属性值

<xref:System.Net.Sockets.NetworkStream>  
`PooledStream` 套接字的网络流。

## <a name="remarks"></a>备注

> [!WARNING]
> `PooledStream.NetworkStream` 属性是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此属性。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（在 System.web 中）

**.NET Framework 版本：** 自2.0 起可用。
