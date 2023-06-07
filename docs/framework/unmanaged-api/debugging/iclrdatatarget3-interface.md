---
title: ICLRDataTarget3 接口
ms.date: 03/30/2017
api_name:
- ICLRDataTarget3
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: d2711bdf-64b3-404c-a0c3-01ba4907f703
topic_type:
- apiref
ms.openlocfilehash: 7297bfa5297878dde6867a99029ac88754a05290
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723579"
---
# <a name="iclrdatatarget3-interface"></a>ICLRDataTarget3 接口

提供对异常信息的访问的 [ICLRDataTarget2](iclrdatatarget2-interface.md) 的子类。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetExceptionRecord 方法](iclrdatatarget3-getexceptionrecord-method.md)|由公共语言运行时 (CLR) 数据访问服务调用，以检索与目标进程关联的异常记录。|  
|[GetExceptionContextRecord 方法](iclrdatatarget3-getexceptioncontextrecord-method.md)|由公共语言运行时 (CLR) 数据访问服务调用，以检索与目标进程关联的上下文记录。|  
|[GetExceptionThreadID 方法](iclrdatatarget3-getexceptionthreadid-method.md)|由 CLR 数据访问服务调用，从而获取引发异常的线程的 ID。|  
  
## <a name="remarks"></a>注解  

 API 客户端（即调试器）必须针对特定的目标进程实现此接口。 例如，活动进程的实现将不同于内存转储的。 目标可能不支持对其内存区域的修改。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[v451_update](../../../../includes/net-current-v451-nov-plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget 接口](iclrdatatarget-interface.md)
- [ICLRDataTarget2 接口](iclrdatatarget2-interface.md)
- [调试接口](debugging-interfaces.md)
