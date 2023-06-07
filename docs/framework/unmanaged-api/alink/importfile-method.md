---
title: ImportFile 方法
ms.date: 03/30/2017
api_name:
- IALink.ImportFile
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ImportFile
helpviewer_keywords:
- ImportFile method
ms.assetid: bcbe321f-b83a-4e9a-9f10-8d913e244dc9
topic_type:
- apiref
ms.openlocfilehash: f30307884a268008fd4d1a8de31ec5a49b6ab92d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705236"
---
# <a name="importfile-method"></a>ImportFile 方法

导入程序集和未绑定模块。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ImportFile(  
    LPCWSTR pszFilename,  
    LPCWSTR pszTargetName,  
    BOOL fSmartImport,  
    mdToken* pImportToken,  
    IMetaDataAssemblyImport** ppAssemblyScope,  
    DWORD* pdwCountOfScopes  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `pszFilename`  
 要导入的文件的完全限定名称。  
  
 `pszTargetName`  
 可选输出文件名，可用于在文件链接到程序集时对其进行重命名。  
  
 `fSmartImport`  
 如果为 TRUE，则使用 ImportTypes，否则必须手动执行导入。  
  
 `pImportToken`  
 指向将存储唯一文件 ID 的标记的指针。 该文件可以是程序集或文件。  
  
 `ppAssemblyScope`  
 接收指向 [IMetaDataAssemblyImport 接口](../metadata/imetadataassemblyimport-interface.md)的指针。 如果文件不是程序集，则可以为 NULL。  
  
 `pdwCountOfScopes`  
 一个指针，指向已导入的文件和/或范围的计数。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
