---
title: ICLRHostBindingPolicyManager::EvaluatePolicy 方法
ms.date: 03/30/2017
api_name:
- ICLRHostBindingPolicyManager.EvaluatePolicy
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRHostBindingPolicyManager::EvaluatePolicy
helpviewer_keywords:
- ICLRHostBindingPolicyManager::EvaluatePolicy method [.NET Framework hosting]
- EvaluatePolicy method [.NET Framework hosting]
ms.assetid: 3a3a9446-7a4e-4836-9b27-5c536c15993d
topic_type:
- apiref
ms.openlocfilehash: 9840217abdf8b3e1d0917b7447572b6860c181c8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720303"
---
# <a name="iclrhostbindingpolicymanagerevaluatepolicy-method"></a>ICLRHostBindingPolicyManager::EvaluatePolicy 方法

代表主机计算绑定策略。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EvaluatePolicy (  
    [in] LPCWSTR     pwzReferenceIdentity,  
    [in] BYTE       *pbApplicationPolicy,  
    [in] DWORD       cbAppPolicySize,  
    [out, size_is(*pcchPostPolicyReferenceIdentity)] LPWSTR pwzPostPolicyReferenceIdentity,  
    [in, out] DWORD *pcchPostPolicyReferenceIdentity,  
    [out] DWORD     *pdwPoliciesApplied  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzReferenceIdentity`  
 中在策略计算之前对程序集的引用。  
  
 `pbApplicationPolicy`  
 中指向包含策略数据的缓冲区的指针。  
  
 `cbAppPolicySize`  
 中缓冲区的大小 `pbApplicationPolicy` 。  
  
 `pwzPostPolicyReferenceIdentity`  
 弄对新策略数据进行计算后对程序集的引用。  
  
 `pcchPostPolicyReferenceIdentity`  
 [in，out]一个指针，指向对新策略数据进行计算后的程序集标识引用缓冲区的大小。  
  
 `pdwPoliciesApplied`  
 弄指向 [EBindPolicyLevels](ebindpolicylevels-enumeration.md) 值的逻辑或组合的指针，指示已应用的策略。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|评估已成功完成。|  
|E_INVALIDARG|`pwzReferenceIdentity`或 `pbApplicationPolicy` 为空引用。|  
|ERROR_INSUFFICIENT_BUFFER|`cbAppPolicySize` 太小。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 此 `EvaluatePolicy` 方法允许主机影响绑定策略，以维护特定于主机的程序集版本要求。 策略引擎本身仍保留在 CLR 内。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRHostBindingPolicyManager 接口](iclrhostbindingpolicymanager-interface.md)
