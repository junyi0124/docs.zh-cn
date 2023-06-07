---
title: IMetaDataImport::GetClassLayout 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetClassLayout
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetClassLayout
helpviewer_keywords:
- IMetaDataImport::GetClassLayout method [.NET Framework metadata]
- GetClassLayout method, IMetaDataImport interface [.NET Framework metadata]
ms.assetid: 8f35414d-f40b-4b99-8768-9adb675c622a
topic_type:
- apiref
ms.openlocfilehash: 5a442d8d0916b0e86f25c03507de66fc999f2159
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711248"
---
# <a name="imetadataimportgetclasslayout-method"></a>IMetaDataImport::GetClassLayout 方法

获取指定的 TypeDef 标记所引用类的布局信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetClassLayout  (
   [in]  mdTypeDef          td,
   [out] DWORD              *pdwPackSize,  
   [out] COR_FIELD_OFFSET   rFieldOffset[],  
   [in]  ULONG              cMax,  
   [out] ULONG              *pcFieldOffset,  
   [out] ULONG              *pulClassSize  
);  
```  
  
## <a name="parameters"></a>参数  

 `td`  
 中具有要返回的布局的类的 TypeDef 标记。  
  
 `pdwPackSize`  
 弄值1、2、4、8或16，表示类的 pack 大小。  
  
 `rFieldOffset`  
 弄 [COR_FIELD_OFFSET](cor-field-offset-structure.md) 值的数组。  
  
 `cMax`  
 [in] `rFieldOffset` 数组的最大大小。  
  
 `pcFieldOffset`  
 弄中返回的元素的数目 `rFieldOffset` 。  
  
 `pulClassSize`  
 弄所表示的类的大小（以字节为单位） `td` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
