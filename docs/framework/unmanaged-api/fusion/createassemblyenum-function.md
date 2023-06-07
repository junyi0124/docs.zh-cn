---
title: CreateAssemblyEnum 函数
ms.date: 03/30/2017
api_name:
- CreateAssemblyEnum
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- DLLExport
f1_keywords:
- CreateAssemblyEnum
helpviewer_keywords:
- CreateAssemblyEnum function [.NET Framework fusion]
ms.assetid: 3506df38-6cea-42f6-946e-4287863bcfb3
topic_type:
- apiref
ms.openlocfilehash: b7e3696121475885f5061bd96eb6905d7ccae734
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683168"
---
# <a name="createassemblyenum-function"></a>CreateAssemblyEnum 函数

获取一个指针，该指针指向可使用指定的[IAssemblyName](iassemblyname-interface.md)枚举程序集中的对象的[IAssemblyEnum](iassemblyenum-interface.md)实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateAssemblyEnum (  
    [out] IAssemblyEnum  **pEnum,  
    [in]  IUnknown       *pUnkReserved,  
    [in]  IAssemblyName  *pName,  
    [in]  DWORD          dwFlags,  
    [in]  LPVOID         pvReserved  
 );  
```  
  
## <a name="parameters"></a>参数  

 `pEnum`  
 弄指向包含所请求指针的内存位置的指针 `IAssemblyEnum` 。  
  
 `pUnkReserved`  
 中保留以供将来进行扩展。 `pUnkReserved` 必须为空引用。  
  
 `pName`  
 中 `IAssemblyName` 请求的程序集的。 此名称用于筛选枚举。 它可以为 null，以枚举全局程序集缓存中的所有程序集。  
  
 `dwFlags`  
 中用于修改枚举器的行为的标志。 此参数仅包含 [ASM_CACHE_FLAGS](asm-cache-flags-enumeration.md) 枚举中的一个位。  
  
 `pvReserved`  
 中保留以供将来进行扩展。 `pvReserved` 必须为空引用。  
  
## <a name="remarks"></a>注解  

 `dwFlags`参数只包含枚举中的一个位 `ASM_CACHE_FLAGS` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyEnum 接口](iassemblyenum-interface.md)
- [IAssemblyName 接口](iassemblyname-interface.md)
- [合成全局静态函数](fusion-global-static-functions.md)
