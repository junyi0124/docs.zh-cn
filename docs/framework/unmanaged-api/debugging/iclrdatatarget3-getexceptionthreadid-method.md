---
title: ICLRDataTarget3::GetExceptionThreadID 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICLRDataTarget3.GetExceptionThreadID
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: 307d6ac7-4a86-45f3-999d-6b47004a68f2
topic_type:
- apiref
ms.openlocfilehash: 0a8b7a90cd909379f870f6a501a940386d2e1451
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723592"
---
# <a name="iclrdatatarget3getexceptionthreadid-method"></a>ICLRDataTarget3::GetExceptionThreadID 方法

由公共语言运行时 (CLR) 数据访问服务调用，从而获取引发异常的线程的 ID。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetExceptionThreadID(  
    [out] ULONG32* threadID  
);  
```  
  
## <a name="parameters"></a>参数  

 `threadID`  
 [out] 引发异常的线程的 ID。  
  
## <a name="return-value"></a>返回值  

 如果成功，则返回值是 `S_OK`；如果失败，则返回失败 `HRESULT` 代码。 `HRESULT` 代码可以包括但不限于以下代码：  
  
|返回代码|描述|  
|-----------------|-----------------|  
|`S_OK`|方法成功。|  
|`HRESULT_FROM_WIN32(ERROR_NOT_FOUND)`|找不到该异常的有效线程 ID。|  
  
## <a name="remarks"></a>注解  

 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[v451_update](../../../../includes/net-current-v451-nov-plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget3 接口](iclrdatatarget3-interface.md)
- [GetExceptionContextRecord 方法](iclrdatatarget3-getexceptioncontextrecord-method.md)
- [GetExceptionRecord 方法](iclrdatatarget3-getexceptionrecord-method.md)
