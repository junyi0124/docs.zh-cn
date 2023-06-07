---
title: ICorDebugProcess4：:P rocessStateChanged 方法
ms.date: 02/07/2019
api_name:
- ICorDebugProcess4::ProcessStateChanged
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess4::ProcessStateChanged
helpviewer_keywords:
- ICorDebugProcess4::ProcessStateChanged method [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: 910c411d2c63ce2c6cf262e28e08546657dc2a4c
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83213564"
---
# <a name="icordebugprocess4processstatechanged-method"></a>ICorDebugProcess4：:P rocessStateChanged 方法

通知 ICorDebug 管道进程外调试器正在继续执行调试对象的执行。

## <a name="syntax"></a>语法

```cpp
HRESULT ProcessStateChanged(
    [in] CorDebugStateChange change
);
```

## <a name="parameters"></a>参数

 `eChange`\
中描述进程的执行状态更改的[CorDebugStateChange 枚举](cordebugstatechange-enumeration.md)的成员。

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `ICorDebugProcess4` ，并且对应于虚拟方法表的第四个槽。

## <a name="requirements"></a>要求

 **平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

 **标头：** 内容

 **库：** 内容

 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>请参阅

- [ICorDebugProcess4 接口](icordebugprocess4-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
