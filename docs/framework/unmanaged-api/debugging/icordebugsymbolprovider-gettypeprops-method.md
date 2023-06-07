---
title: ICorDebugSymbolProvider::GetTypeProps 方法
ms.date: 03/30/2017
ms.assetid: 35ac4140-91ea-4c77-b1c4-1daf41986ca5
ms.openlocfilehash: 4738d35aabbc2197c796405e0657607f75ff685d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95694498"
---
# <a name="icordebugsymbolprovidergettypeprops-method"></a>ICorDebugSymbolProvider::GetTypeProps 方法

给定 vtable 中的相对虚拟地址 (RVA)，返回类型的属性信息（例如其泛型参数的签名数量）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetTypeProps(  
   [in]  ULONG32 vtableRva,  
   [in]  ULONG32 cbSignature,  
   [out] ULONG32 *pcbSignature,  
   [out, size_is(cbSignature), length_is(*pcbSignature)] BYTE signature[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `tableRva`  
 [in] vtable 中的相对虚拟地址 (RVA)。  
  
 `cbSignature`  
 [in] `signature` 数组的大小。 请参阅“备注”部分。  
  
 `pcbSignature`  
 [out] [out] 指向返回的 `signature` 数组的大小的指针。  
  
 `signature`  
 [out] 保存所有泛型参数的 typespec 签名的缓冲区。  
  
## <a name="remarks"></a>注解  

 若要获取类型的数组的所需大小 `signature` ，请将 `cbSignature` 参数设置为0，将设置 `signature` 为 **null**。 当该方法返回时，`pcbSignature` 将包含 `signature` 数组所需的字节数。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetMethodProps 方法](icordebugsymbolprovider-getmethodprops-method.md)
- [ICorDebugSymbolProvider 接口](icordebugsymbolprovider-interface.md)
- [调试接口](debugging-interfaces.md)
