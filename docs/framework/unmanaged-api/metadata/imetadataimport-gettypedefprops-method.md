---
title: IMetaDataImport::GetTypeDefProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetTypeDefProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetTypeDefProps
helpviewer_keywords:
- GetTypeDefProps method [.NET Framework metadata]
- IMetaDataImport::GetTypeDefProps method [.NET Framework metadata]
ms.assetid: 00061a25-ba05-47a7-b984-fd916b06b149
topic_type:
- apiref
ms.openlocfilehash: 9dd973fe3e0802c49c220db51a21c223730e5aec
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729156"
---
# <a name="imetadataimportgettypedefprops-method"></a>IMetaDataImport::GetTypeDefProps 方法

返回 <xref:System.Type> 由指定的 TypeDef 标记所表示的的元数据信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetTypeDefProps (  
   [in]  mdTypeDef   td,  
   [out] LPWSTR      szTypeDef,  
   [in]  ULONG       cchTypeDef,  
   [out] ULONG       *pchTypeDef,  
   [out] DWORD       *pdwTypeDefFlags,  
   [out] mdToken     *ptkExtends  
);  
```  
  
## <a name="parameters"></a>参数  

 `td`  
 中TypeDef 标记，它表示要为其返回元数据的类型。  
  
 `szTypeDef`  
 弄包含类型名称的缓冲区。  
  
 `cchTypeDef`  
 中的大小（以宽字符为大小） `szTypeDef` 。  
  
 `pchTypeDef`  
 弄返回的宽字符数 `szTypeDef` 。  
  
 `pdwTypeDefFlags`  
 弄指向修改类型定义的任何标志的指针。 此值是 [CorTypeAttr](cortypeattr-enumeration.md) 枚举中的位掩码。  
  
 `ptkExtends`  
 弄一个 TypeDef 或 TypeRef 元数据标记，它表示所请求类型的基类型。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
