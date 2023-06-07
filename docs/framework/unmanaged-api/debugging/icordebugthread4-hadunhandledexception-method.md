---
title: ICorDebugThread4::HadUnhandledException 方法
ms.date: 03/30/2017
api_name:
- ICorDebugThread4.HadUnhandledException Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread4::HadUnhandledException
helpviewer_keywords:
- ICorDebugThread4::HadUnhandledException method [.NET Framework debugging]
- HadUnhandledException method [.NET Framework debugging]
ms.assetid: 05558daa-39e2-4c38-aeaf-e2aec4a09468
topic_type:
- apiref
ms.openlocfilehash: 4e368b2c63e8e43b5c392bec4b79daac8bae249d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678527"
---
# <a name="icordebugthread4hadunhandledexception-method"></a>ICorDebugThread4::HadUnhandledException 方法

指示线程是否曾经出现过未经处理的异常。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetBlockingObjects (  
    [out] ICorDebugBlockingObjectEnum **ppBlockingObjectEnum  
    );  
```  
  
## <a name="parameters"></a>参数  

 `ppBlockingObjectEnum`  
 弄指向 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构的有序枚举的地址的指针。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|线程在创建后有一个未经处理的异常。|  
|S_FALSE|线程从未有未经处理的异常。|  
  
## <a name="remarks"></a>注解  

 此方法指示线程是否曾经出现过未经处理的异常。 当触发未经处理的异常回调或启动本机 JIT 附加时，此方法将保证返回 S_OK。 不保证 [ICorDebugThread. GetCurrentException](icordebugthread-getcurrentexception-method.md) 方法将返回未经处理的异常;但是，如果在获取未经处理的异常回调或本机 JIT 附加时该进程尚未继续，则会出现此错误。 而且， (可能不太可能) 在触发本机 JIT 附加时，有多个线程出现未处理的异常。 在这种情况下，无法确定哪个异常触发了 JIT 附加。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugThread4 接口](icordebugthread4-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
