---
title: IMetaDataImport::GetPinvokeMap 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetPinvokeMap
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetPinvokeMap
helpviewer_keywords:
- IMetaDataImport::GetPinvokeMap method [.NET Framework metadata]
- GetPinvokeMap method [.NET Framework metadata]
ms.assetid: b8685c1e-b80c-4198-8eb3-748d6f48a99e
topic_type:
- apiref
ms.openlocfilehash: c34215f48190e60bd1a851f31b8b23f09491f4e1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729234"
---
# <a name="imetadataimportgetpinvokemap-method"></a>IMetaDataImport::GetPinvokeMap 方法

获取用于表示 PInvoke 调用的目标程序集的 ModuleRef 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetPinvokeMap (  
   [in]  mdToken       tk,  
   [out] DWORD         *pdwMappingFlags,  
   [out] LPWSTR        szImportName,  
   [in]  ULONG         cchImportName,  
   [out] ULONG         *pchImportName,  
   [out] mdModuleRef   *pmrImportDLL  
);  
```  
  
## <a name="parameters"></a>参数  

 `tk`  
 中要为其获取 PInvoke 映射元数据的 FieldDef 或 MethodDef 标记。  
  
 `pdwMappingFlags`  
 弄一个指针，指向用于映射的标志。 此值是 [CorPinvokeMap](corpinvokemap-enumeration.md) 枚举中的位掩码。  
  
 `szImportName`  
 弄非托管目标 DLL 的名称。  
  
 `cchImportName`  
 中的大小（以宽字符为大小） `szImportName` 。  
  
 `pchImportName`  
 弄返回的宽字符数 `szImportName` 。  
  
 `pmrImportDLL`  
 弄一个指针，指向表示非托管目标对象库的 ModuleRef 标记。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
