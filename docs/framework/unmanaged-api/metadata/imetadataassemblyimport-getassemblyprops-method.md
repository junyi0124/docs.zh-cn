---
title: IMetaDataAssemblyImport::GetAssemblyProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetAssemblyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetAssemblyProps
helpviewer_keywords:
- GetAssemblyProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetAssemblyProps method [.NET Framework metadata]
ms.assetid: 0eaa4aa9-9441-444a-920c-e4b2a2db899e
topic_type:
- apiref
ms.openlocfilehash: 1e1a86cdf55812197aae653dca256fb910a7f168
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733888"
---
# <a name="imetadataassemblyimportgetassemblyprops-method"></a>IMetaDataAssemblyImport::GetAssemblyProps 方法

获取具有指定元数据签名的程序集的属性集。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAssemblyProps (  
    [in]  mdAssembly          mda,  
    [out] const void          **ppbPublicKey,
    [out] ULONG               *pcbPublicKey,  
    [out] ULONG               *pulHashAlgId,  
    [out] LPWSTR              szName,  
    [in] ULONG                cchName,  
    [out] ULONG               *pchName,  
    [out] ASSEMBLYMETADATA    *pMetaData,  
    [out] DWORD               *pdwAssemblyFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `mda`  
 [in]。 `mdAssembly`表示要获取其属性的程序集的元数据标记。  
  
 `ppbPublicKey`  
 弄指向公钥或元数据标记的指针。  
  
 `pcbPublicKey`  
 弄返回的公钥中的字节数。  
  
 `pulHashAlgId`  
 弄一个指针，指向用于对程序集中的文件进行哈希处理的算法。  
  
 `szName`  
 弄程序集的简单名称。  
  
 `cchName`  
 中的大小（宽字符） `szName` 。  
  
 `pchName`  
 弄中实际返回的宽字符数 `szName` 。  
  
 `pMetaData`  
 弄指向 ASSEMBLYMETADATA 结构的指针，该结构包含程序集元数据。  
  
 `pdwAssemblyFlags`  
 弄描述应用于程序集的元数据的标志。 此值是一个或多个 [CorAssemblyFlags](corassemblyflags-enumeration.md) 值的组合。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyImport 接口](imetadataassemblyimport-interface.md)
