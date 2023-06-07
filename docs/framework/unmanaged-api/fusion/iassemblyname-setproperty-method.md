---
title: IAssemblyName::SetProperty 方法
ms.date: 03/30/2017
api_name:
- IAssemblyName.SetProperty
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyName::SetProperty
helpviewer_keywords:
- IAssemblyName::SetProperty method [.NET Framework fusion]
- SetProperty method [.NET Framework fusion]
ms.assetid: 496c3add-f60b-4073-943f-d1bcf33330cb
topic_type:
- apiref
ms.openlocfilehash: 04b3e73e2166efb2ec0821d21da3da4c53b0ca4b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95688648"
---
# <a name="iassemblynamesetproperty-method"></a>IAssemblyName::SetProperty 方法

设置由指定的属性标识符引用的属性的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetProperty (  
    [in] DWORD  PropertyId,  
    [in] LPVOID pvProperty,  
    [in] DWORD  cbProperty  
);  
```  
  
## <a name="parameters"></a>参数  

 `PropertyId`  
 中将设置其值的属性的唯一标识符。  
  
 `pvProperty`  
 中要将引用的属性设置为的值 `PropertyId` 。  
  
 `cbProperty`  
 中的大小（以字节为单位） `pvProperty` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyName 接口](iassemblyname-interface.md)
