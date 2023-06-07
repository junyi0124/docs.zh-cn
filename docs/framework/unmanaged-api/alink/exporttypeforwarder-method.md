---
title: ExportTypeForwarder 方法
ms.date: 03/30/2017
api_name:
- IALink.ExportTypeForwarder
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ExportTypeForwarder
helpviewer_keywords:
- ExportTypeForwarder method
ms.assetid: 55989fa9-ab43-4f08-8eb6-2eb56fa7ca76
topic_type:
- apiref
ms.openlocfilehash: 4e6ceabf37056bfc25247266be2c7801cb0e13e1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684767"
---
# <a name="exporttypeforwarder-method"></a>ExportTypeForwarder 方法

将类型转发器添加到给定程序集的类型表。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ExportTypeForwarder(  
    mdAssemblyRef   tkAssemblyRef,  
    LPCWSTR         pszTypename,  
    DWORD           dwFlags,  
    mdExportedType* pType  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `tkAssemblyRef`  
 对类型转发器所引用的程序集的引用。  
  
 `pszTypename`  
 要导出的完全限定的类型名称。  
  
 `dwFlags`  
 `ComType` 标志，如 `tdPublic` 或 `tdNested` 。 此值可传递给 [DefineExportedType 方法](../metadata/imetadataassemblyemit-defineexportedtype-method.md)。  
  
 `pType`  
 接收导出类型的标记。 这仅适用于发出嵌套类型。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
