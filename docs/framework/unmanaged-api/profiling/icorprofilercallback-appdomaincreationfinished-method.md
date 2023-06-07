---
title: ICorProfilerCallback::AppDomainCreationFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AppDomainCreationFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AppDomainCreationFinished
helpviewer_keywords:
- AppDomainCreationFinished method [.NET Framework profiling]
- ICorProfilerCallback::AppDomainCreationFinished method [.NET Framework profiling]
ms.assetid: dbab7d90-d515-4dc9-8195-294d5d04bab6
topic_type:
- apiref
ms.openlocfilehash: 688b9975cc68463de066e5225c6ab1e04cbb5337
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685365"
---
# <a name="icorprofilercallbackappdomaincreationfinished-method"></a>ICorProfilerCallback::AppDomainCreationFinished 方法

通知探查器已创建应用程序域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT AppDomainCreationFinished(  
    [in] AppDomainID appDomainId,  
    [in] HRESULT     hrStatus);
```  
  
## <a name="parameters"></a>参数

- `appDomainId`

  \[中的] 标识已创建的域。

- `hrStatus`

  \[in] 一个 HRESULT，指示是否已成功完成创建应用程序域。

## <a name="remarks"></a>注解  

 在调用方法之前，应用程序 ID 对于任何信息请求均无效 `AppDomainCreationFinished` 。  
  
 在回调后，某些加载应用程序域的部分可能会继续 `AppDomainCreationFinished` 。 中的 HRESULT 失败 `hrStatus` 表示失败。 但是，中的成功 HRESULT `hrStatus` 仅指示创建应用程序域的第一部分已成功。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
