---
title: ICorDebugMDA::GetXML 方法
ms.date: 03/30/2017
api_name:
- ICorDebugMDA.GetXML
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugMDA::GetXML
helpviewer_keywords:
- ICorDebugMDA::GetXML method [.NET Framework debugging]
- GetXML method [.NET Framework debugging]
ms.assetid: 29746b24-3766-4255-8813-0426c45e73e5
topic_type:
- apiref
ms.openlocfilehash: 9a088c7e4e9c72c8247ccdd384bc724587210c37
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710865"
---
# <a name="icordebugmdagetxml-method"></a>ICorDebugMDA::GetXML 方法

获取与托管调试助手关联的完整 XML 流 (MDA) 表示的 [ICorDebugMDA](icordebugmda-interface.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetXML (  
    [in] ULONG32   cchName,  
    [out] ULONG32  *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
        WCHAR      szName[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `cchName`  
 [in] `szName` 数组的大小。  
  
 `pcchName`  
 弄指向 XML 流长度的指针。  
  
 `szName`  
 弄要在其中存储 XML 流的数组。 该数组可能为空。  
  
## <a name="remarks"></a>注解  

 此 `GetXML` 方法可能会影响性能，具体取决于关联的 XML 流的大小。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMDA 接口](icordebugmda-interface.md)
- [使用托管调试助手诊断错误](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
