---
title: ImportFileEx2 方法
ms.date: 03/30/2017
api_name:
- IALink2.ImportFileEx2
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ImportFileEx2
helpviewer_keywords:
- ImportFileEx2 method
ms.assetid: 02c789fd-16fc-48c6-9619-56e87e2a37ca
topic_type:
- apiref
ms.openlocfilehash: 59149e79e926a0b9a3e549e013bf178e54ddf6fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705171"
---
# <a name="importfileex2-method"></a>ImportFileEx2 方法

导入程序集和未绑定模块。 此方法类似于 [ImportFile 方法](importfile-method.md)，但即使磁盘上没有要导入的文件也能正常工作。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ImportFileEx2(  
    LPCWSTR pszFilename,  
    LPCWSTR pszTargetName,  
    IMetaDataAssemblyImport* pAssemblyScopeIn,  
    BOOL fSmartImport,  
    DWORD dwOpenFlags,  
    mdToken* pImportToken,  
    IMetaDataAssemblyImport** ppAssemblyScope,  
    DWORD* pdwCountOfScopes  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `pszFilename`  
 要导入的文件的名称。  
  
 `pszTargetName`  
 目标文件的可选名称。  
  
 `pAssemblyScopeIn`  
 可选的导入范围 [IMetaDataAssemblyImport 接口](../metadata/imetadataassemblyimport-interface.md) 接口。  
  
 `fSmartImport`  
 如果为 TRUE，则使用 ImportTypes，否则必须手动执行导入。  
  
 `dwOpenFlags`  
 要传递给 [OpenScope 方法](../metadata/imetadatadispenser-openscope-method.md)的标志。  
  
 `pImportToken`  
 接收程序集或文件的唯一 ID。  
  
 `ppAssemblyScope`  
 接收程序集导入范围 [IMetaDataAssemblyImport 接口](../metadata/imetadataassemblyimport-interface.md) 接口。 如果文件不是程序集，则可以为 NULL。  
  
 `pdwCountOfScopes`  
 接收导入的文件和/或范围的数量。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink。  
  
## <a name="see-also"></a>另请参阅

- [IALink2 接口](ialink2-interface.md)
- [IALink 接口](ialink-interface.md)
- [ALink API](index.md)
