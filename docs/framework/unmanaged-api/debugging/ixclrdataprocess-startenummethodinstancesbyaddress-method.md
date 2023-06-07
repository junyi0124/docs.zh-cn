---
title: IXCLRDataProcess：： StartEnumMethodInstancesByAddress 方法
ms.date: 01/16/2019
api.name:
- IXCLRDataProcess::StartEnumMethodInstancesByAddress Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess::StartEnumMethodInstancesByAddress Method
helpviewer.keywords:
- IXCLRDataProcess::StartEnumMethodInstancesByAddress Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 52d533f1c86b34b7b9febade8590e7ab58d8221e
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420729"
---
# <a name="ixclrdataprocessstartenummethodinstancesbyaddress-method"></a>IXCLRDataProcess：： StartEnumMethodInstancesByAddress 方法

提供用于枚举 `AppDomain` 从给定地址开始的方法实例的句柄。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT StartEnumMethodInstancesByAddress(
    [in] CLRDATA_ADDRESS     address,
    [in] IXCLRDataAppDomain *appDomain,
    [out] CLRDATA_ENUM      *handle
);
```

## <a name="parameters"></a>参数

`address`\
中第一个方法实例的地址。

`appDomain`\
中方法实例的 AppDomain。

`handle`\
弄枚举方法实例的句柄。

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `IXCLRDataProcess` ，并且对应于虚拟方法表的第28个槽。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [CLRDataSourceType 枚举](clrdatasourcetype-enumeration.md)
- [调试](index.md)
- [IXCLRDataProcess 接口](ixclrdataprocess-interface.md)
