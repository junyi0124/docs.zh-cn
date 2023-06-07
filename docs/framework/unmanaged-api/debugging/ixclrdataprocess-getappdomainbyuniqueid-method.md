---
title: IXCLRDataProcess：： GetAppDomainByUniqueId 方法
ms.date: 01/16/2019
api.name:
- IXCLRDataProcess::GetAppDomainByUniqueId Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess::GetAppDomainByUniqueId Method
helpviewer.keywords:
- IXCLRDataProcess::GetAppDomainByUniqueId Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: bb02ffed09cbcc31e653bfd3165050c247908c5d
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420774"
---
# <a name="ixclrdataprocessgetappdomainbyuniqueid-method"></a>IXCLRDataProcess：： GetAppDomainByUniqueId 方法

`AppDomain`基于其唯一标识符，在进程中获取。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT GetAppDomainByUniqueID(
    [in] ULONG64               id,
    [out] IXCLRDataAppDomain **appDomain
);
```

## <a name="parameters"></a>参数

`id`\
中AppDomain 的唯一标识符

`appDomain`\
弄AppDomain

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `IXCLRDataProcess` ，并且对应于虚拟方法表的第20个槽。 `IXCLRDataAppDomain*`返回的用于与其他 api 交互。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。
**标头：** 无**库：** 无 **.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [IXCLRDataProcess 接口](ixclrdataprocess-interface.md)
