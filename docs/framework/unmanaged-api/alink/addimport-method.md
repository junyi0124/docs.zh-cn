---
title: AddImport 方法
ms.date: 03/30/2017
api_name:
- AddImport
- IALink.AddImport
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- AddImport
helpviewer_keywords:
- AddImport method
ms.assetid: 4fedf8a0-08c8-43d0-aa00-20f2a521c991
topic_type:
- apiref
ms.openlocfilehash: cf73ada36be66edb3fa267d61873ae9acb088a34
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717040"
---
# <a name="addimport-method"></a>AddImport 方法

将导入添加到程序集。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT AddImport(  
    mdAssembly      AssemblyID,  
    mdToken         ImportToken,  
    DWORD           dwFlags,  
    mdFile*         pFileToken  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `AssemblyID`  
 要扩充的程序集的唯一 ID。  
  
 `ImportToken`  
 要导入的文件从 [ImportFile 方法](importfile-method.md)检索的唯一 ID。  
  
 `dwFlags`  
 COM + FileDef 标志 `ffContainsNoMetaData` ，例如和 `ffWriteable` 。 `dwFlags` 传递给 [DefineFile 方法](../metadata/imetadataassemblyemit-definefile-method.md)。  
  
 `pFileToken`  
 指向接收结果文件的 ID 的标记的指针。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
