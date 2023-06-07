---
title: ICorDebugFunction3 接口
ms.date: 03/30/2017
api_name:
- ICorDebugFunction3
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: b22717b9-ead5-4eea-887e-789b52d613dc
topic_type:
- apiref
ms.openlocfilehash: 17eda7470e5f2e4b41d1f2ed164843eaeeedea93
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695864"
---
# <a name="icordebugfunction3-interface"></a>ICorDebugFunction3 接口

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 对 ICorDebugFunction 接口进行逻辑扩展，以提供对 ReJIT 请求中的代码的访问。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetActiveReJitRequestILCode 方法](icordebugfunction3-getactiverejitrequestilcode-method.md)|获取一个接口指针，该指针指向包含活动 ReJIT 请求中的 IL 的 [ICorDebugILCode](icordebugilcode-interface.md) 。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
- [ReJIT： How-To 指南](/archive/blogs/davbr/rejit-a-how-to-guide)
