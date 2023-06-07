---
title: ICLRProbingAssemblyEnum::Get 方法
ms.date: 03/30/2017
api_name:
- ICLRProbingAssemblyEnum.Get
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRProbingAssemblyEnum::Get
helpviewer_keywords:
- Get method, ICLRProbingAssemblyEnum interface [.NET Framework hosting]
- ICLRProbingAssemblyEnum::Get method [.NET Framework hosting]
ms.assetid: fdb67a77-782f-44cf-a8a1-b75999b0f3c8
topic_type:
- apiref
ms.openlocfilehash: 9a6145ff2874890f052f18a7e537e20ff259933c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728935"
---
# <a name="iclrprobingassemblyenumget-method"></a>ICLRProbingAssemblyEnum::Get 方法

获取指定索引处的程序集标识。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Get (  
    [in] DWORD dwIndex,  
    [out, size_is(*pcchBufferSize)] LPWSTR pwzBuffer,  
    [in, out] DWORD *pcchBufferSize  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwIndex`  
 中要返回的程序集标识的从零开始的索引。  
  
 `pwzBuffer`  
 弄包含程序集标识数据的缓冲区。  
  
 `pcchBufferSize`  
 [in，out]缓冲区的大小 `pwzBuffer` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`Get` 已成功返回。|  
|ERROR_INSUFFICIENT_BUFFER|`pwzBuffer` 太小。|  
|ERROR_NO_MORE_ITEMS|枚举中没有更多的项。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 如果方法返回 E_FAIL，则 CLR 在该进程内将不再可用。 对任何宿主方法的后续调用都会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 索引0处的标识是特定于处理器体系结构的标识。 位于索引1处的标识是适用于 Microsoft 中间语言 (MSIL) 的特定于体系结构的程序集。 索引2处的标识不包含体系结构信息。  
  
 `Get` 通常称为两次。 第一次调用为提供 null 值 `pwzBuffer` ，并将设置 `pcchBufferSize` 为适合的大小 `pwzBuffer` 。 第二个调用提供适当大小的 `pwzBuffer` ，并在完成时包含规范程序集标识数据。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRProbingAssemblyEnum 接口](iclrprobingassemblyenum-interface.md)
- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
