---
title: ICorDebugNativeFrame::GetLocalRegisterMemoryValue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame.GetLocalRegisterMemoryValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame::GetLocalRegisterMemoryValue
helpviewer_keywords:
- ICorDebugNativeFrame::GetLocalRegisterMemoryValue method [.NET Framework debugging]
- GetLocalRegisterMemoryValue method [.NET Framework debugging]
ms.assetid: d350f69d-9aff-4f5a-8301-daea22dee2da
topic_type:
- apiref
ms.openlocfilehash: 4b77ad2f31f10bd14ce7d8242a584da737428344
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709293"
---
# <a name="icordebugnativeframegetlocalregistermemoryvalue-method"></a>ICorDebugNativeFrame::GetLocalRegisterMemoryValue 方法

获取参数或局部变量的值，该参数或局部变量的下限字和高位字分别存储在内存位置和指定的寄存器中（对于此本机帧）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetLocalRegisterMemoryValue (  
    [in] CorDebugRegister   highWordReg,  
    [in] CORDB_ADDRESS      lowWordAddress,  
    [in] ULONG              cbSigBlob,  
    [in] PCCOR_SIGNATURE    pvSigBlob,  
    [out] ICorDebugValue    **ppValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `highWordReg`  
 中"CorDebugRegister" 枚举的一个值，它指定包含值高位字的寄存器。  
  
 `lowWordAddress`  
 中一个 `CORDB_ADDRESS` 值，该值指定包含值的低位字的内存位置。  
  
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
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
