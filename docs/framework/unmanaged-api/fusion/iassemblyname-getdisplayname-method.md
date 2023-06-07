---
title: IAssemblyName::GetDisplayName 方法
ms.date: 03/30/2017
api_name:
- IAssemblyName.GetDisplayName
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyName::GetDisplayName
helpviewer_keywords:
- GetDisplayName method, IAssemblyName interface [.NET Framework fusion]
- IAssemblyName::GetDisplayName method [.NET Framework fusion]
ms.assetid: 9a26547a-9a34-4284-a463-78a7d4b496cf
topic_type:
- apiref
ms.openlocfilehash: 13d71a9da2539c45076e5eb92e153e37c1df4b47
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727908"
---
# <a name="iassemblynamegetdisplayname-method"></a>IAssemblyName::GetDisplayName 方法

获取此 [IAssemblyName](iassemblyname-interface.md) 对象所引用的程序集的可读名称。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetDisplayName (  
        [out]      LPOLESTR  szDisplayName,  
        [in, out]  LPDWORD   pccDisplayName,  
        [in]       DWORD     dwDisplayFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `szDisplayName`  
 弄包含所引用程序集的名称的字符串缓冲区。  
  
 `pccDisplayName`  
 [in，out] `szDisplayName` 宽字符的大小，包括 null 终止符。  
  
 `dwDisplayFlags`  
 中影响的功能 [ASM_DISPLAY_FLAGS](asm-display-flags-enumeration.md) 值的按位组合 `szDisplayName` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyName 接口](iassemblyname-interface.md)
- [合成枚举](fusion-enumerations.md)
