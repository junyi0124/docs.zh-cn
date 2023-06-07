---
title: SetAssemblyFile 方法
ms.date: 03/30/2017
api_name:
- IALink.SetAssemblyFile
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- SetAssemblyFile
helpviewer_keywords:
- SetAssemblyFile method
ms.assetid: 3a912787-f139-43ca-a841-8bbda3107ecf
topic_type:
- apiref
ms.openlocfilehash: 45eed17b91f70d4188d1d89fc91a41455f3e845b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732640"
---
# <a name="setassemblyfile-method"></a>SetAssemblyFile 方法

指定要生成的程序集的名称。 不在生成未绑定的模块时使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetAssemblyFile(  
    LPCWSTR pszFilename,  
    IMetaDataEmit* pEmitter,  
    AssemblyFlags afFlags,  
    mdAssembly* pAssemblyID  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `pszFilename`  
 清单文件的完全限定名称。  
  
 `pEmitter`  
 指向 [IMetaDataEmit 接口](../metadata/imetadataemit-interface.md) 接口的指针。  
  
 `afFlags`  
 [AssemblyFlags 枚举](../metadata/assemblyflags-enumeration.md)中定义的标志。  
  
 `pAssemblyID`  
 指向生成的程序集的 ID 的指针。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink。  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
