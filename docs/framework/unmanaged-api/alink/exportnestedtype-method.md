---
title: ExportNestedType 方法
ms.date: 03/30/2017
api_name:
- IALink.ExportNestedType
- ExportNestedType
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ExportNestedType
helpviewer_keywords:
- ExportNestedType method
ms.assetid: dec7df60-4d30-47c8-99db-72e0419e5f76
topic_type:
- apiref
ms.openlocfilehash: 69c99e2facfcb9077c3fc4131186ba3882c7cef6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684832"
---
# <a name="exportnestedtype-method"></a>ExportNestedType 方法

指定嵌套类型为可导出。 [ExportType 方法](exporttype-method.md)还可以导出嵌套类型，但此方法的速度更快。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ExportNestedType(  
    mdAssembly      AssemblyID,  
    mdToken         FileToken,  
    mdTypeDef       TypeToken,  
    mdExportedType  ParentType,  
    LPCWSTR         pszTypename,  
    DWORD           dwFlags,  
    mdExportedType* pType  
) PURE;
```  
  
## <a name="parameters"></a>参数  

 `AssemblyID`  
 要从中导出的程序集的 ID。  
  
 `FileToken`  
 定义要导出的类型的文件标记或文件的程序集。  
  
 `TypeToken`  
 要使其可导出的类型的类型标记。  
  
 `ParentType`  
 父类型的标记。  
  
 `pszTypename`  
 要导出的完全限定的类型名称。  
  
 `dwFlags`  
 `ComType` 标志，如 `tdPublic` 或 `tdNested` 。 此值可传递给 [DefineExportedType 方法](../metadata/imetadataassemblyemit-defineexportedtype-method.md)。  
  
 `pType`  
 接收导出类型的令牌。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
