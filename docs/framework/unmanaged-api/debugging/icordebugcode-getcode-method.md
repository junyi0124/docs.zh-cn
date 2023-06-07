---
title: ICorDebugCode::GetCode 方法
ms.date: 03/30/2017
api_name:
- ICorDebugCode.GetCode
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode::GetCode
helpviewer_keywords:
- ICorDebugCode::GetCode method [.NET Framework debugging]
- GetCode method, ICorDebugCode interface [.NET Framework debugging]
ms.assetid: 7137e3d1-1dad-48d8-8c37-16ac816534d3
topic_type:
- apiref
ms.openlocfilehash: 20eac75a1f1d13b6a30267d56ff66024725e6f33
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674770"
---
# <a name="icordebugcodegetcode-method"></a>ICorDebugCode::GetCode 方法

获取用于反汇编的指定函数的所有代码。 此方法在 .NET Framework 版本2.0 中已弃用。 改 [为使用 ICorDebugCode2：： GetCodeChunks](icordebugcode2-getcodechunks-method.md) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCode (  
    [in] ULONG32     startOffset,
    [in] ULONG32     endOffset,  
    [in] ULONG32     cBufferAlloc,  
    [out, size_is(cBufferAlloc),  
        length_is(*pcBufferSize)] BYTE buffer[],  
    [out] ULONG32    *pcBufferSize  
);  
```  
  
## <a name="parameters"></a>参数  

 `startOffset`  
 中函数开头的偏移量。  
  
 `endOffset`  
 中函数末尾的偏移量。  
  
 `cBufferAlloc`  
 中 `buffer` 将返回代码的数组的大小。  
  
 `buffer`  
 弄将向其中返回代码的数组。  
  
 `pcBufferSize`  
 弄返回的字节数。  
  
## <a name="remarks"></a>注解  

 如果函数的代码已划分为多个块，则它们将按照增加的本机偏移量的顺序进行连接。 不检查指令边界。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1.1、1。0  
  
## <a name="see-also"></a>另请参阅

- [GetCodeChunks 方法](icordebugcode2-getcodechunks-method.md)
