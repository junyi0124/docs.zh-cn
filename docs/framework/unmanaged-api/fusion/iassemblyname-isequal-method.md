---
title: IAssemblyName::IsEqual 方法
ms.date: 03/30/2017
api_name:
- IAssemblyName.IsEqual
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyName::IsEqual
helpviewer_keywords:
- IsEqual method [.NET Framework fusion]
- IAssemblyName::IsEqual method [.NET Framework fusion]
ms.assetid: 6dfc220f-d0d4-45b3-bfce-5829f817766f
topic_type:
- apiref
ms.openlocfilehash: 0fabf8159c2626d4e1716e3be60baaf1ec834032
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712971"
---
# <a name="iassemblynameisequal-method"></a>IAssemblyName::IsEqual 方法

根据指定的比较标志，确定指定的 [IAssemblyName](iassemblyname-interface.md) 对象是否等于此 `IAssemblyName` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT IsEqual (  
    [in] IAssemblyName  *pName,  
    [in] DWORD          dwCmpFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `pName`  
 中要与 `IAssemblyName` 此进行比较的对象 `IAssemblyName` 。  
  
 `dwCmpFlags`  
 中影响比较的 [ASM_CMP_FLAGS](asm-cmp-flags-enumeration.md) 值的按位组合。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyName 接口](iassemblyname-interface.md)
- [合成枚举](fusion-enumerations.md)
