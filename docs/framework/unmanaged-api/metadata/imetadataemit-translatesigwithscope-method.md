---
title: IMetaDataEmit::TranslateSigWithScope 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.TranslateSigWithScope
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::TranslateSigWithScope
helpviewer_keywords:
- TranslateSigWithScope method [.NET Framework metadata]
- IMetaDataEmit::TranslateSigWithScope method [.NET Framework metadata]
ms.assetid: 47915d33-b7bf-409e-b484-4ee1df15de22
topic_type:
- apiref
ms.openlocfilehash: 80d33da2eb2a7f0cfbe5dcb7279fff9973dada2e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712919"
---
# <a name="imetadataemittranslatesigwithscope-method"></a>IMetaDataEmit::TranslateSigWithScope 方法

将程序集导入到当前作用域中，并为合并的作用域获取新的元数据签名。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT TranslateSigWithScope (
    [in]  IMetaDataAssemblyImport   *pAssemImport,
    [in]  const void                *pbHashValue,
    [in]  ULONG                     cbHashValue,
    [in]  IMetaDataImport           *import,
    [in]  PCCOR_SIGNATURE           pbSigBlob,
    [in]  ULONG                     cbSigBlob,  
    [in]  IMetaDataAssemblyEmit     *pAssemEmit,
    [in]  IMetaDataEmit             *emit,
    [out] PCOR_SIGNATURE            pvTranslatedSig,
    [in]  ULONG                     cbTranslatedSigMax,
    [out] ULONG                     *pcbTranslatedSig
);  
```  
  
## <a name="parameters"></a>参数  

 `pAssemImport`  
 中导入程序集 (的接口) 在其中定义签名。  
  
 `pbHashValue`  
 中程序集的哈希 blob。  
  
 `cbHashValue`  
 中中的字节数 `pbHashValue` 。  
  
 `import`  
 中导入元数据范围的接口。  
  
 `pbSigBlob`  
 中要导入的签名。  
  
 `cbSigBlob`  
 中的大小（以字节为单位） `pbSigBlob` 。  
  
 `pAssemEmit`  
 中导出程序集的接口。  
  
 `emit`  
 中导出元数据范围的接口。  
  
 `pvTranslatedSig`  
 弄用于保存已转换的签名 blob 的缓冲区。  
  
 `cbTranslatedSigMax`  
 中的容量（以字节为单位） `pvTranslatedSig` 。  
  
 `pcbTranslatedSig`  
 弄已翻译签名中的实际字节数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
- [IMetaDataAssemblyImport 接口](imetadataassemblyimport-interface.md)
- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
