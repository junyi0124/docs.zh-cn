---
title: ICorPublish::GetProcess 方法
ms.date: 03/30/2017
api_name:
- ICorPublish.GetProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublish::GetProcess
helpviewer_keywords:
- ICorPublish::GetProcess method [.NET Framework debugging]
- GetProcess method, ICorPublish interface [.NET Framework debugging]
ms.assetid: c5143805-2eb7-45b8-85ed-c8fb34df1084
topic_type:
- apiref
ms.openlocfilehash: a45f613b7547e2e80abdbd8ac85cb0b2b6a499e5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716884"
---
# <a name="icorpublishgetprocess-method"></a>ICorPublish::GetProcess 方法

获取一个表示具有指定标识符的进程的 [ICorPublishProcess](icorpublishprocess-interface.md) 实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetProcess(  
    [in] unsigned              pid,
    [out] ICorPublishProcess   **ppProcess  
);  
```  
  
## <a name="parameters"></a>参数  

 `pid`  
 中进程的标识符。  
  
 `ppProcess`  
 弄一个指针，指向 `ICorPublishProcess` 表示进程的实例的地址。  
  
## <a name="remarks"></a>注解  

 `GetProcess` 如果进程不存在，或者不是可由当前用户调试的托管进程，则会失败。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CorPub，CorPub  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorPublish 接口](icorpublish-interface.md)
