---
title: ICorDebugNativeFrame::GetLocalDoubleRegisterValue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame.GetLocalDoubleRegisterValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame::GetLocalDoubleRegisterValue
helpviewer_keywords:
- ICorDebugNativeFrame::GetLocalDoubleRegisterValue method [.NET Framework debugging]
- GetLocalDoubleRegisterValue method [.NET Framework debugging]
ms.assetid: 1f838215-ac8a-434f-8ce6-03021d3098d9
topic_type:
- apiref
ms.openlocfilehash: 10f06fb04099ef947711bc7c5641e5a7f1fa36b7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695694"
---
# <a name="icordebugnativeframegetlocaldoubleregistervalue-method"></a>ICorDebugNativeFrame::GetLocalDoubleRegisterValue 方法

获取此本机帧存储在两个指定寄存器中的参数或局部变量的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetLocalDoubleRegisterValue (  
    [in] CorDebugRegister   highWordReg,  
    [in] CorDebugRegister   lowWordReg,  
    [in] ULONG              cbSigBlob,  
    [in] PCCOR_SIGNATURE    pvSigBlob,  
    [out] ICorDebugValue    **ppValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `highWordReg`  
 中"CorDebugRegister" 枚举的一个值，它指定包含值高位字的寄存器。  
  
 `lowWordReg`  
 中枚举的一个值 `CorDebugRegister` ，该值指定包含值的低位字的寄存器。  
  
 `cbSigBlob`  
 中一个整数，指定参数引用的二进制元数据签名的大小 `pvSigBlob` 。  
  
 `pvSigBlob`  
 中一个 `PCCOR_SIGNATURE` 值，该值指向值类型的二进制元数据签名。  
  
 `ppValue`  
 弄一个指向 "ICorDebugValue" 对象地址的指针，该对象表示存储在指定寄存器中的检索到的值。  
  
## <a name="remarks"></a>注解  

 `GetLocalDoubleRegisterValue`方法既可以在本机框架中使用，也可以在实时 (JIT) 编译的框架中使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
