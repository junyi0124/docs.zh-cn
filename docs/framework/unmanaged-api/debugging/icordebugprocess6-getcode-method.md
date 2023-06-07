---
title: ICorDebugProcess6::GetCode 方法
ms.date: 03/30/2017
ms.assetid: faa538c2-60c9-4064-b996-1b4c24ebd751
ms.openlocfilehash: cee1556fd7d803765b09a7cbd86ce2ff7be91cc9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732627"
---
# <a name="icordebugprocess6getcode-method"></a>ICorDebugProcess6::GetCode 方法

获取特定代码地址上的托管代码的相关信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCode(  
    [in] CORDB_ADDRESS codeAddress,
    [out] ICorDebugCode **ppCode);  
```  
  
## <a name="parameters"></a>参数  

 `codeAddress`  
 中一个 [CORDB_ADDRESS](../common-data-types-unmanaged-api-reference.md) 值，该值指定托管代码段的起始地址。  
  
 `ppCode`  
 弄指向表示托管代码段的 "ICorDebugCode" 对象地址的指针。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试进程6”接口](icordebugprocess6-interface.md)
- [调试接口](debugging-interfaces.md)
