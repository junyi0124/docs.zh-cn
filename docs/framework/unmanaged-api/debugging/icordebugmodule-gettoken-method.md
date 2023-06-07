---
title: ICorDebugModule::GetToken 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule.GetToken
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule::GetToken
helpviewer_keywords:
- ICorDebugModule::GetToken method [.NET Framework debugging]
- GetToken method, ICorDebugModule interface [.NET Framework debugging]
ms.assetid: f759f87a-18ae-4c1a-8300-29b803432d0a
topic_type:
- apiref
ms.openlocfilehash: 6ffc74247a4ecafcc3744923c0def99220b5ca6f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709877"
---
# <a name="icordebugmodulegettoken-method"></a>ICorDebugModule::GetToken 方法

获取此模块的表项的标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetToken(  
    [out] mdModule *pToken  
);  
```  
  
## <a name="parameters"></a>参数  

 `pToken`  
 弄指向 `mdModule` 引用模块元数据的标记的指针。  
  
## <a name="remarks"></a>注解  

 该令牌可以传递到 [IMetaDataImport](../metadata/imetadataimport-interface.md)、 [IMetaDataImport2](../metadata/imetadataimport2-interface.md)和 [IMetaDataAssemblyImport](../metadata/imetadataassemblyimport-interface.md) 元数据导入接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- Metadata 
