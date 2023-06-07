---
title: ICorDebugController::Continue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugController.Continue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::Continue
helpviewer_keywords:
- Continue method [.NET Framework debugging]
- ICorDebugController::Continue method [.NET Framework debugging]
ms.assetid: 8684cd06-ad3e-48ef-832e-15320e1f43a2
topic_type:
- apiref
ms.openlocfilehash: 0fd7dfc1a48e21abbc80692c110bee55beb68e6b
ms.sourcegitcommit: 957c49696eaf048c284ef8f9f8ffeb562357ad95
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82892854"
---
# <a name="icordebugcontrollercontinue-method"></a>ICorDebugController::Continue 方法

在调用[Stop 方法](icordebugcontroller-stop-method.md)后继续执行托管线程。

## <a name="syntax"></a>语法

```cpp
HRESULT Continue (
    [in] BOOL fIsOutOfBand
);
```

## <a name="parameters"></a>参数

`fIsOutOfBand`  
中如果从`true`带外事件继续，则设置为;否则，将设置`false`为。

## <a name="remarks"></a>备注

`Continue`在调用`ICorDebugController::Stop`方法后继续此过程。

在执行混合模式调试时，不要在 Win32 `Continue`事件线程上调用，除非从带外事件继续。

*带内事件*是托管事件或普通非托管事件，在此期间，调试器支持与进程的托管状态交互。 在这种情况下，调试器接收[ICorDebugUnmanagedCallback：:D ebugevent](icordebugunmanagedcallback-debugevent-method.md)回调，其`fOutOfBand`参数设置为`false`。

带*外事件*是一种非托管事件，在此过程中，由于发生事件，进程停止时无法与进程的托管状态交互。 在这种情况下，调试器会`ICorDebugUnmanagedCallback::DebugEvent`接收回叫`fOutOfBand` ，其参数`true`设置为。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
