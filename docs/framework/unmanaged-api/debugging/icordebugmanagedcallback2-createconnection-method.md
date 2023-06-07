---
title: ICorDebugManagedCallback2::CreateConnection 方法
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback2.CreateConnection
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback2::CreateConnection
helpviewer_keywords:
- CreateConnection method [.NET Framework debugging]
- ICorDebugManagedCallback2::CreateConnection method [.NET Framework debugging]
ms.assetid: 49e647be-9d63-4250-9d11-704e2a400d1b
topic_type:
- apiref
ms.openlocfilehash: 5a4ebf65dfaaa487e87f3fd78e54c468c7e24a89
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697228"
---
# <a name="icordebugmanagedcallback2createconnection-method"></a>ICorDebugManagedCallback2::CreateConnection 方法

通知调试器已创建新连接。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateConnection (  
    [in] ICorDebugProcess     *pProcess,  
    [in] CONNID               dwConnectionId,  
    [in] WCHAR                *pConnName  
);  
```  
  
## <a name="parameters"></a>参数  

 `pProcess`  
 中一个指向 "ICorDebugProcess" 对象的指针，该对象表示在其中创建连接的进程  
  
 `dwConnectionId`  
 中新连接的 ID。  
  
 `pConnName`  
 中指向新连接名称的指针。  
  
## <a name="remarks"></a>注解  

 `CreateConnection`在以下任一情况下，都将激发回调：  
  
- 调试器附加到包含连接的进程时。 在这种情况下，运行时将 `CreateConnection` 为进程中的每个连接生成并调度一个事件和一个 [ICorDebugManagedCallback2：： ChangeConnection](icordebugmanagedcallback2-changeconnection-method.md) 事件。  
  
- 当主机在[托管 API](../hosting/index.md)中调用[ICLRDebugManager：： BeginConnection](../hosting/iclrdebugmanager-beginconnection-method.md)时。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugManagedCallback2 接口](icordebugmanagedcallback2-interface.md)
- [ICorDebugManagedCallback 接口](icordebugmanagedcallback-interface.md)
