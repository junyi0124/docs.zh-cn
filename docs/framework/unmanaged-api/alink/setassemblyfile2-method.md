---
title: SetAssemblyFile2 方法
ms.date: 03/30/2017
api_name:
- IALink2.SetAssemblyFile2
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- SetAssemblyFile2
helpviewer_keywords:
- SetAssemblyFile2 method
ms.assetid: eedb9125-1ef1-4000-abfc-7de86e5a1f17
topic_type:
- apiref
ms.openlocfilehash: 131f5d951e524ef48f2cfe1e3e88ef80ac21c452
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703676"
---
# <a name="setassemblyfile2-method"></a>SetAssemblyFile2 方法

为新的程序集设置和选项的名称。 生成未绑定的模块时，请勿调用此方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetAssemblyFile2(  
    LPCWSTR pszFilename,  
    IMetaDataEmit2* pEmitter,  
    AssemblyFlags   afFlags,  
    mdAssembly* pAssemblyID  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `pszFilename`  
 清单文件的名称。  
  
 `pEmitter`  
 此文件的[IMetaDataEmit2 接口](../metadata/imetadataemit2-interface.md)接口。  
  
 `afFlags`  
 [AssemblyFlags 枚举](../metadata/assemblyflags-enumeration.md)表示的选项。  
  
 `pAssemblyID`  
 接收正在构造的程序集的唯一 ID。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink。  
  
## <a name="see-also"></a>另请参阅

- [IALink2 接口](ialink2-interface.md)
- [IALink 接口](ialink-interface.md)
- [ALink API](index.md)
