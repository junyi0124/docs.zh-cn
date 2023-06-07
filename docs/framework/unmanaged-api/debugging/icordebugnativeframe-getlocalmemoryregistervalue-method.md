---
title: ICorDebugNativeFrame::GetLocalMemoryRegisterValue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame.GetLocalMemoryRegisterValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame::GetLocalMemoryRegisterValue
helpviewer_keywords:
- ICorDebugNativeFrame::GetLocalMemoryRegisterValue method [.NET Framework debugging]
- GetLocalMemoryRegisterValue method
ms.assetid: 33a19f6e-1029-4d53-af64-19591c6e58ee
topic_type:
- apiref
ms.openlocfilehash: 15485ac94ed9074baacc4fd2662a04bdcefcf1e7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709313"
---
# <a name="icordebugnativeframegetlocalmemoryregistervalue-method"></a>ICorDebugNativeFrame::GetLocalMemoryRegisterValue 方法

获取参数或本地变量的值，在此本机帧中，将低字和高位字分别存储在指定的寄存器和内存位置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetLocalMemoryRegisterValue (  
    [in] CORDB_ADDRESS      highWordAddress,  
    [in] CorDebugRegister   lowWordRegister,  
    [in] ULONG              cbSigBlob,  
    [in] PCCOR_SIGNATURE    pvSigBlob,  
    [out] ICorDebugValue    **ppValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `highWordAddress`  
 中一个 `CORDB_ADDRESS` 值，该值指定包含值高位字的内存位置。  
  
 `lowWordRegister`  
 中"CorDebugRegister" 枚举的一个值，它指定包含值的低位字的寄存器。  
  
 `cbSigBlob`  
 中一个整数，指定参数引用的二进制元数据签名的大小 `pvSigBlob` 。  
  
 `pvSigBlob`  
 中一个 `PCCOR_SIGNATURE` 值，该值指向值类型的二进制元数据签名。  
  
 `ppValue`  
 弄一个指向 "ICorDebugValue" 对象地址的指针，该对象表示存储在指定寄存器和内存位置的检索到的值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
