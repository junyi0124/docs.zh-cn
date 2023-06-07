---
title: HttpStatusDescription 类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.HttpStatusDescription
- System.Net.HttpStatusDescription.Get
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 0214d8259c735de11abe204680d619a7298466de
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990443"
---
# <a name="httpstatusdescription-class"></a>HttpStatusDescription 类

提供标准 HTTP 状态说明。 无法继承此类。

```csharp
internal static class HttpStatusDescription
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="get-method"></a>Get 方法

返回与指定的 HTTP 状态代码相关联的说明。

```csharp
internal static string Get(int code)
```

### <a name="parameters"></a>参数

`code` <xref:System.Int32>

HTTP 状态代码，例如 `404` 。

### <a name="return-value"></a>返回值

<xref:System.String?displayProperty=nameWithType>

HTTP 状态说明。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
