---
title: ICorDebugMergedAssemblyRecord::GetPublicKeyToken 方法
ms.date: 03/30/2017
ms.assetid: 72020b72-9611-4bc3-b1e7-5a16b023bfa3
ms.openlocfilehash: c642d8af7e84288d3aa8912372a2f169b8f22503
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710566"
---
# <a name="icordebugmergedassemblyrecordgetpublickeytoken-method"></a>ICorDebugMergedAssemblyRecord::GetPublicKeyToken 方法

获取程序集的公钥标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetPublicKeyToken(  
   [in] ULONG32 cbPublicKeyToken,
   [out] ULONG32 *pcbPublicKeyToken,
   [out, size_is(cbPublicKeyToken), length_is(*pcbPublicKeyToken)] BYTE pbPublicKeyToken[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `cbPublicKeyToken`  
 [in] `pbPublicKeyToken` 数组中的最大字节数。  
  
 `pcbPublicKeyToken`  
 [out] 指向写入 `pbPublicKeyToken` 数组的实际字节数的指针。  
  
 `pbPublicKeyToken`  
 [out] 指向包含程序集公钥标记的字节数组的指针。  
  
## <a name="remarks"></a>注解  

 程序集的公钥标记是指其公钥中 SHA1 哈希的最后 8 个字节。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMergedAssemblyRecord 接口](icordebugmergedassemblyrecord-interface.md)
- [调试接口](debugging-interfaces.md)
