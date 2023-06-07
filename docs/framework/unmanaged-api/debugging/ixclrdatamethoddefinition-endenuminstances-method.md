---
title: IXCLRDataMethodDefinition：： EndEnumInstances 方法
ms.date: 01/16/2019
api.name:
- IXCLRDataMethodDefinition::EndEnumInstances Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataMethodDefinition::EndEnumInstances Method
helpviewer.keywords:
- IXCLRDataMethodDefinition::EndEnumInstances Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 7271c9594a679af205c404f59ff6731821aa0acf
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420989"
---
# <a name="ixclrdatamethoddefinitionendenuminstances-method"></a>IXCLRDataMethodDefinition：： EndEnumInstances 方法

释放实例枚举期间使用的内部迭代器所使用的资源。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT EndEnumInstances(
    [in] CLRDATA_ENUM handle
);
```

## <a name="parameters"></a>参数

`handle`\
弄用于枚举实例的句柄。

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `IXCLRDataMethodDefinition` ，并且对应于虚拟方法表的第7个槽。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [IXCLRDataMethodDefinition 接口](ixclrdatamethoddefinition-interface.md)
