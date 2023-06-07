---
title: IMetaDataImport::GetMethodProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetMethodProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetMethodProps
helpviewer_keywords:
- GetMethodProps method [.NET Framework metadata]
- IMetaDataImport::GetMethodProps method [.NET Framework metadata]
ms.assetid: e0667ef7-1d31-4c89-a2d3-d426f023f8d2
topic_type:
- apiref
ms.openlocfilehash: 0eb4e9d713581cf32cec18bb02a6bd13542e517a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733175"
---
# <a name="imetadataimportgetmethodprops-method"></a>IMetaDataImport::GetMethodProps 方法

获取与指定的 MethodDef 标记引用的方法关联的元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMethodProps (  
    [in]  mdMethodDef         mb,  
    [out] mdTypeDef           *pClass,  
    [out] LPWSTR              szMethod,  
    [in]  ULONG               cchMethod,  
    [out] ULONG               *pchMethod,  
    [out] DWORD               *pdwAttr,  
    [out] PCCOR_SIGNATURE     *ppvSigBlob,  
    [out] ULONG               *pcbSigBlob,  
    [out] ULONG               *pulCodeRVA,  
    [out] DWORD               *pdwImplFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `mb`  
 中MethodDef 标记，它表示要为其返回元数据的方法。  
  
 `pClass`  
 弄一个指针，指向表示实现方法的类型的 TypeDef 标记。  
  
 `szMethod`  
 弄指向包含方法名称的缓冲区的指针。  
  
 `cchMethod`  
 中请求的大小 `szMethod` 。  
  
 `pchMethod`  
 弄一个指针，它指向 `szMethod` 方法名称中实际的宽字符数（以宽字符为范围），或在截断的情况下为。  
  
 `pdwAttr`  
 弄一个指针，指向与方法关联的任何标志。  
  
 `ppvSigBlob`  
 弄指向方法的二进制元数据签名的指针。  
  
 `pcbSigBlob`  
 弄一个指针，它指向的大小（以字节为单位） `ppvSigBlob` 。  
  
 `pulCodeRVA`  
 弄指向方法的相对虚拟地址的指针。  
  
 `pdwImplFlags`  
 弄一个指针，指向方法的任何实现标志。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
