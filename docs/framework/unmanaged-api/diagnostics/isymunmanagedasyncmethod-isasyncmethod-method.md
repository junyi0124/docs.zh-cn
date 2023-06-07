---
title: ISymUnmanagedAsyncMethod::IsAsyncMethod 方法
ms.date: 03/30/2017
ms.assetid: 670a7653-dac6-4171-98ee-d669e3adf4b2
ms.openlocfilehash: af02aba1a0d390c103e8c6108f90b93fe2a98ff3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707147"
---
# <a name="isymunmanagedasyncmethodisasyncmethod-method"></a>ISymUnmanagedAsyncMethod::IsAsyncMethod 方法

检查方法是否有异步信息。  
  
 如果此方法返回，则 `FALSE` 调用此接口中的任何其他方法无效。 `E_UNEXPECTED`在这种情况下，它们都将返回。  
  
## <a name="syntax"></a>语法  
  
```idl  
HRESULT IsAsyncMethod(    [out, retval] BOOL* pRetVal);  
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|---------------|-----------------|  
|`pRetVal`||  
  
## <a name="return-value"></a>返回值  

 返回 `HRESULT`。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedAsyncMethod 接口](isymunmanagedasyncmethod-interface.md)
