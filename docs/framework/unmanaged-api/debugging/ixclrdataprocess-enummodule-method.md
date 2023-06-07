---
title: IXCLRDataProcess：： EnumModule 方法
ms.date: 01/16/2019
api.name:
- IXCLRDataProcess::EnumModule Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess::EnumModule Method
helpviewer.keywords:
- IXCLRDataProcess::EnumModule Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 5caadcfe091393a8ff79106d57a50a532c349829
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420768"
---
# <a name="ixclrdataprocessenummodule-method"></a>IXCLRDataProcess：： EnumModule 方法

枚举此进程的模块。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT EnumModule(
    [in, out] CLRDATA_ENUM  *handle,
    [out] IXCLRDataModule  **mod
);
```

## <a name="parameters"></a>参数

`handle`\
[in，out]用于枚举模块的句柄。

`mod`\
弄枚举的模块。

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `IXCLRDataProcess` ，并且对应于虚拟方法表的第25个槽。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [CLRDataSourceType 枚举](clrdatasourcetype-enumeration.md)
- [调试](index.md)
- [IXCLRDataModule 接口](ixclrdatamodule-interface.md)
- [IXCLRDataProcess 接口](ixclrdataprocess-interface.md)
