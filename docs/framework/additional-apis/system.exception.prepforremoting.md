---
title: PrepForRemoting 方法（系统）
description: 查看 .NET 中的 PrepForRemoting 方法。 方法将服务器端堆栈跟踪添加到消息中，然后在客户端重新引发异常。
ms.date: 10/08/2019
topic_type:
- apiref
api_name:
- System.Exception.PrepForRemoting
api_location:
- mscorlib.dll
api_type:
- Assembly
ms.openlocfilehash: 9ceb73499ae3bb308975e6db5b961bfe40165ba3
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105265"
---
# <a name="exceptionprepforremoting-method"></a>Exception.PrepForRemoting 方法

保留服务器端堆栈跟踪，方法是将其追加到消息，然后在客户端调用站点重新引发异常。

```csharp
internal Exception PrepForRemoting();
```

## <a name="returns"></a>返回

<xref:System.Exception>  
此 <xref:System.Exception> 实例。

## <a name="remarks"></a>备注

> [!WARNING]
> `Exception.PrepForRemoting`方法是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此方法。

## <a name="requirements"></a>要求

**命名空间：** <xref:System>

**程序集：** mscorlib.dll （在 mscorlib.dll 中）

**.NET Framework 版本：** 自1.0 起可用。
