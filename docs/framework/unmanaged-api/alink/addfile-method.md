---
title: AddFile 方法
ms.date: 03/30/2017
api_name:
- IALink.AddFile
- AddFile
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- AddFile
helpviewer_keywords:
- AddFile method
ms.assetid: 9e707abb-f905-4568-9356-12aa21d1b11c
topic_type:
- apiref
ms.openlocfilehash: 53ca4005f5681cfc5d550301d8aad1406aceb3a2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717196"
---
# <a name="addfile-method"></a>AddFile 方法

将文件添加到程序集。 还可用于创建未绑定的模块。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT AddFile(  
    mdAssembly      AssemblyID,  
    LPCWSTR         pszFilename,  
    DWORD           dwFlags,  
    IMetaDataEmit*  pEmitter,  
    mdFile*         pFileToken  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `AssemblyID`  
 要扩充的程序集的唯一 ID。  
  
 `pszFilename`  
 要添加的文件的完全限定名称。  
  
 `dwFlags`  
 COM + FileDef 标志 `ffContainsNoMetaData` ，例如和 `ffWriteable` 。 `dwFlags` 传递给 [DefineFile 方法](../metadata/imetadataassemblyemit-definefile-method.md)。  
  
 `pEmitter`  
 要用于发出元数据的[IMetaDataEmit 接口](../metadata/imetadataemit-interface.md)接口（如有必要）。  
  
 `pFileToken`  
 一个指针，指向将存储所添加文件的唯一 ID 的位置。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink。  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
