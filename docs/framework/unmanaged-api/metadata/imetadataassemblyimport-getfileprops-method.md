---
title: IMetaDataAssemblyImport::GetFileProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetFileProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetFileProps
helpviewer_keywords:
- GetFileProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetFileProps method [.NET Framework metadata]
ms.assetid: c5e6216f-ae3d-4697-9688-66b69c1251ec
topic_type:
- apiref
ms.openlocfilehash: 0b9ff2716cc0bc32c81fe6fcdd4e6c367d4d835f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718171"
---
# <a name="imetadataassemblyimportgetfileprops-method"></a>IMetaDataAssemblyImport::GetFileProps 方法

获取具有指定的元数据签名的文件的属性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFileProps (  
    [in]  mdFile      mdf,
    [out] LPWSTR      szName,
    [in]  ULONG       cchName,
    [out] ULONG       *pchName,
    [out] const void  **ppbHashValue,
    [out] ULONG       *pcbHashValue,
    [out] DWORD       *pdwFileFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `mdf`  
 中 `mdFile` 表示要获取其属性的文件的元数据标记。  
  
 `szName`  
 弄文件的简单名称。  
  
 `cchName`  
 中的大小（宽字符） `szName` 。  
  
 `pchName`  
 弄中实际返回的宽字符数 `szName` 。  
  
 `ppbHashValue`  
 弄指向哈希值的指针。 这是文件的哈希，使用 SHA-1 算法。  
  
 `pcbHashValue`  
 弄返回的哈希值中的宽字符数。  
  
 `pdwFileFlags`  
 弄一个指针，指向描述应用于文件的元数据的标志。 Flags 值是一个或多个 [CorFileFlags](corfileflags-enumeration.md) 值的组合。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyImport 接口](imetadataassemblyimport-interface.md)
