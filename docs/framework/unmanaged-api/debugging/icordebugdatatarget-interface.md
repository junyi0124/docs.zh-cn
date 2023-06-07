---
title: ICorDebugDataTarget 接口
ms.date: 03/30/2017
api_name:
- ICorDebugDataTarget
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugDataTarget
helpviewer_keywords:
- ICorDebugDataTarget interface [.NET Framework debugging]
ms.assetid: df5f05be-bed7-4f3c-bc89-dbb435d79a0b
topic_type:
- apiref
ms.openlocfilehash: 14f0b247ded363dedce193886aab50538db3e6a6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683675"
---
# <a name="icordebugdatatarget-interface"></a>ICorDebugDataTarget 接口

提供一个回调接口，该接口可提供对特定目标进程的访问。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetPlatform 方法](icordebugdatatarget-getplatform-method.md)|提供有关平台的信息，包括在其上运行目标进程的处理器体系结构和操作系统。|  
|[ReadVirtual 方法](icordebugdatatarget-readvirtual-method.md)|获取从指定地址开始的连续内存块，并将其返回到提供的缓冲区中。|  
|[GetThreadContext 方法](icordebugdatatarget-getthreadcontext-method.md)|请求指定线程的当前线程上下文。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugDataTarget` 及其方法具有以下特征：  
  
- 调试服务调用此接口上的方法以访问目标进程中的内存和其他数据。  
  
- 调试器客户端必须根据特定目标 (（例如，实时进程或内存转储) ）实现此接口。  
  
- 只能 `ICorDebugDataTarget` 从其他接口实现的方法中调用方法 `ICorDebug*` 。 这确保了调试器客户端可以控制调用它的线程和时间。  
  
- `ICorDebugDataTarget`实现必须始终返回有关目标的最新信息。  
  
 当 `ICorDebug*` 接口 (，因此调用) 的方法时，目标进程应停止并且不以任何方式更改 `ICorDebugDataTarget` 。 如果目标是实时进程并且其状态发生更改，则必须再次调用 [ICLRDebugging：： OpenVirtualProcess](iclrdebugging-openvirtualprocess-method.md) 方法以提供替换 ICorDebugProcess 实例。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
