---
title: IAssemblyEnum::GetNextAssembly 方法
ms.date: 03/30/2017
api_name:
- IAssemblyEnum.GetNextAssembly
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyEnum::GetNextAssembly
helpviewer_keywords:
- GetNextAssembly method [.NET Framework fusion]
- IAssemblyEnum::GetNextAssembly method [.NET Framework fusion]
ms.assetid: 5d7a4ca2-5f46-4ef1-a9a2-257884e9dc11
topic_type:
- apiref
ms.openlocfilehash: af43d9cf4d5aa790036a13d060fc6ccf113f335d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719887"
---
# <a name="iassemblyenumgetnextassembly-method"></a>IAssemblyEnum::GetNextAssembly 方法

获取一个指针，该指针指向此[IAssemblyEnum](iassemblyenum-interface.md)对象中包含的下一个[IAssemblyName](iassemblyname-interface.md) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNextAssembly (  
    [in]  LPVOID          pvReserved,  
    [out] IAssemblyName   **ppName,  
    [in]  DWORD           dwFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `pvReserved`  
 中保留以供将来进行扩展。 `pvReserved` 必须为空引用。  
  
 `ppName`  
 弄返回的 `IAssemblyName` 指针。  
  
 `dwFlags`  
 中保留以供将来进行扩展。 `dwFlags` 必须为 0 (零) 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyName 接口](iassemblyname-interface.md)
- [IAssemblyEnum 接口](iassemblyenum-interface.md)
