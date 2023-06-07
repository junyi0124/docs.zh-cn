---
title: _EFN_GetManagedObjectName 函数
ms.date: 03/30/2017
api_name:
- _EFN_GetManagedObjectName
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- _EFN_GetManagedObjectName
helpviewer_keywords:
- _EFN_GetManagedObjectName function [.NET Framework debugging]
ms.assetid: 6e7c6bee-7ced-495f-bf6c-2a5f0c716f7e
topic_type:
- apiref
ms.openlocfilehash: 0fb694cf85256c0f3ac5ae179e53ff504ab707e9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676200"
---
# <a name="_efn_getmanagedobjectname-function"></a>\_EFN \_ GetManagedObjectName 函数

使用提供的托管对象指针获取类型的名称。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT _EFN_GetManagedObjectName(  
    [in]  PDEBUG_CLIENT  Client,  
    [in]  ULONG64        objAddr,  
    [out] __out_ecount(cbName) PSTR szName,  
    [out] ULONG          cbName  
);  
```  
  
## <a name="parameters"></a>参数  

 `Client`  
 中指向调试客户端的指针。  
  
 `objAddr`  
 中托管对象指针。  
  
 szName  
 弄类型的名称。  
  
 `cbName`  
 弄字符串缓冲区中的可用字符数。  
  
## <a name="remarks"></a>注解  

 如果当前上下文中的线程上没有托管代码，则该函数将返回具有0xa0 的工具值的 HRESULT SOS_E_NOMANAGEDCODE 和错误代码0x1000。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** SOS_Stacktrace。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试全局静态函数](debugging-global-static-functions.md)
