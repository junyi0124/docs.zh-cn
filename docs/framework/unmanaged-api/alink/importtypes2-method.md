---
title: ImportTypes2 方法
ms.date: 03/30/2017
api_name:
- IALink2.ImportTypes2
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ImportTypes2
helpviewer_keywords:
- ImportTypes2 method
ms.assetid: 32f3ba58-9695-41e9-ba58-fd19e45ed396
topic_type:
- apiref
ms.openlocfilehash: 2ec7708edd86b9f2656d0eee434992c3b73200ca
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705041"
---
# <a name="importtypes2-method"></a>ImportTypes2 方法

启动类型的导入。 调用此方法以开始从通过 [ImportFile 方法](importfile-method.md)导入的每个范围导入类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ImportTypes2(  
    mdAssembly AssemblyID,  
    mdToken FileToken,  
    DWORD dwScope,  
    HALINKENUM* phEnum,  
    IMetaDataImport2** ppImportScope,  
    DWORD* pdwCountOfTypes  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `AssemblyID`  
 要导入的程序集的 ID。  
  
 `FileToken`  
 要从其导入的文件的 ID。  
  
 `dwScope`  
 要从中导入的从零开始的作用域。  
  
 `phEnum`  
 接收给定范围内的类型的枚举器句柄。  
  
 `ppImportScope`  
 可以选择接收 [IMetaDataImport2 接口](../metadata/imetadataimport2-interface.md) 接口。  
  
 `pdwCountOfTypes`  
 可以选择接收指定范围内的类型的计数。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink2 接口](ialink2-interface.md)
- [IALink 接口](ialink-interface.md)
- [ALink API](index.md)
