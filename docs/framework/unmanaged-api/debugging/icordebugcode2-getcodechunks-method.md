---
title: ICorDebugCode2::GetCodeChunks 方法
ms.date: 03/30/2017
api_name:
- ICorDebugCode2.GetCodeChunks
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode2::GetCodeChunks
helpviewer_keywords:
- GetCodeChunks method [.NET Framework debugging]
- ICorDebugCode2::GetCodeChunks method [.NET Framework debugging]
ms.assetid: 210a2f02-2678-4555-bc4a-78a0408764c8
topic_type:
- apiref
ms.openlocfilehash: e419ebb6ffd404368baf32e591e08c4a70645127
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73121120"
---
# <a name="icordebugcode2getcodechunks-method"></a>ICorDebugCode2::GetCodeChunks 方法

获取包含此代码对象的代码块。

## <a name="syntax"></a>语法

```cpp
HRESULT GetCodeChunks (
    [in]  ULONG32     cbufSize,
    [out] ULONG32     *pcnumChunks,
    [out, size_is(cbufSize), length_is(*pcnumChunks)]
        CodeChunkInfo chunks[]
);
```

## <a name="parameters"></a>参数

`cbufSize`  
中`chunks` 数组的大小。

`pcnumChunks`  
弄`chunks` 数组中返回的块区数。

`chunks`  
弄"CodeChunkInfo" 结构的数组，其中每个结构都表示一个代码块。 如果 `cbufSize` 的值为0，则此参数可以为 null。

## <a name="remarks"></a>备注

代码块将永远不会重叠，它们将遵循[ICorDebugCode：： GetCode](icordebugcode-getcode-method.md)连接的顺序。 .NET Framework 版本2.0 中的 Microsoft 中间语言（MSIL）代码对象将包含一个代码块。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
