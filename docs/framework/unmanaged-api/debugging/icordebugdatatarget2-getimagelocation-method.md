---
title: ICorDebugDataTarget2::GetImageLocation 方法
ms.date: 03/30/2017
ms.assetid: 696afe71-5852-478d-a33f-b2d2dbc4b91f
ms.openlocfilehash: c909b46a9bb70d23d1cd3a769ac24fcf58479308
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713790"
---
# <a name="icordebugdatatarget2getimagelocation-method"></a>ICorDebugDataTarget2::GetImageLocation 方法

返回模块基址中的模块路径。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetImageLocation(    [in] CORDB_ADDRESS baseAddress,  
    [in] ULONG32 cchName,  
    [out] ULONG32 *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)] WCHAR szName[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `baseAddress`  
 中一个 [CORDB_ADDRESS](../common-data-types-unmanaged-api-reference.md) 值，该值表示模块的基址。  
  
 `cchName`  
 [输入] 缓冲器中将接收模块路径的字符数。  
  
 `pcchName`  
 [输出] 指针指向写入 `szName` 缓冲器的字符数。  
  
 `szName`  
 [输出] 模块路径。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试数据目标2”接口](icordebugdatatarget2-interface.md)
- [调试接口](debugging-interfaces.md)
