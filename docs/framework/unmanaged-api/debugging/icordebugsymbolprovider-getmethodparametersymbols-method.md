---
title: ICorDebugSymbolProvider::GetMethodParameterSymbols 方法
ms.date: 03/30/2017
ms.assetid: 58b7c0b9-f6ad-4b49-b92d-0e421cfd0ec6
ms.openlocfilehash: 95b7b61087f3d3fddab36b7835a633534c223b72
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730833"
---
# <a name="icordebugsymbolprovidergetmethodparametersymbols-method"></a>ICorDebugSymbolProvider::GetMethodParameterSymbols 方法

给定方法的相对虚拟地址 (RVA) 后，获取该方法的参数符号。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMethodParameterSymbols(  
   [in] ULONG32 nativeRVA,  
   [in] ULONG32 cRequestedSymbols,  
   [out] ULONG32 *pcFetchedSymbols,  
   [out, size_is(cRequestedSymbols), length_is(*pcFetchedSymbols)] ICorDebugVariableSymbol *pSymbols[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `nativeRVA`  
 [in] 方法的本机相对虚拟地址。  
  
 `cRequestedSymbols`  
 [in] 请求的本地符号数。  
  
 `pcFetchedSymbols`  
 [out] 一个指针，指向由方法检索的符号的数量。  
  
 `pcFetchedSymbols`  
 弄指向包含方法的本地符号的 [ICorDebugVariableSymbol](icordebugvariablesymbol-interface.md) 数组的指针。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetMethodLocalSymbols 方法](icordebugsymbolprovider-getmethodlocalsymbols-method.md)
- [ICorDebugSymbolProvider 接口](icordebugsymbolprovider-interface.md)
- [调试接口](debugging-interfaces.md)
