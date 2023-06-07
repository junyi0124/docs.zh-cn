---
title: IMetaDataImport::GetParamProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetParamProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetParamProps
helpviewer_keywords:
- IMetaDataImport::GetParamProps method [.NET Framework metadata]
- GetParamProps method [.NET Framework metadata]
ms.assetid: 4d5e5f00-bcab-4f41-b191-176511a186a7
topic_type:
- apiref
ms.openlocfilehash: a16621f4c9b06f049239dc4e2335d70a167dd756
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729260"
---
# <a name="imetadataimportgetparamprops-method"></a>IMetaDataImport::GetParamProps 方法

获取指定的 ParamDef 标记所引用的参数的元数据值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetParamProps (  
   [in]  mdParamDef      tk,  
   [out] mdMethodDef     *pmd,  
   [out] ULONG           *pulSequence,  
   [out] LPWSTR          szName,  
   [in]  ULONG           cchName,  
   [out] ULONG           *pchName,  
   [out] DWORD           *pdwAttr,  
   [out] DWORD           *pdwCPlusTypeFlag,  
   [out] UVCP_CONSTANT   *ppValue,  
   [out] ULONG           *pcchValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `tk`  
 中一个 ParamDef 标记，它表示要为其返回元数据的参数。  
  
 `pmd`  
 弄指向 MethodDef 标记的指针，该标记表示采用参数的方法。  
  
 `pulSequence`  
 弄参数在方法自变量列表中的序号位置。  
  
 `szName`  
 弄用于保存参数名称的缓冲区。  
  
 `cchName`  
 中请求的大小（以宽字符为大小） `szName` 。  
  
 `pchName`  
 弄返回的宽字符大小 `szName` 。  
  
 `pdwAttr`  
 弄指向与参数关联的任何特性标志的指针。 这是一个值的位掩码 `CorParamAttr` 。  
  
 `pdwCPlusTypeFlag`  
 弄指向标志的指针，该标志指定参数为 <xref:System.ValueType> 。  
  
 `ppValue`  
 弄指向参数返回的常量字符串的指针。  
  
 `pcchValue`  
 弄以宽字符为大小的大小 `ppValue` ; 如果不包含字符串，则为零 `ppValue` 。  
  
## <a name="remarks"></a>注解

参数的序列值 `pulSequence` 从1开始。 返回值的序列号为0。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
