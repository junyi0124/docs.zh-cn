---
title: ICLRRuntimeInfo::GetInterface 方法
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.GetInterface
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::GetInterface
helpviewer_keywords:
- GetInterface method [.NET Framework hosting]
- ICLRRuntimeInfo::GetInterface method [.NET Framework hosting]
ms.assetid: cc7b0e5b-48c3-4509-8ebb-611ddb1f7ec2
topic_type:
- apiref
ms.openlocfilehash: 192163ed8af680e39f7f3a03aee3f46546bc7450
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732068"
---
# <a name="iclrruntimeinfogetinterface-method"></a>ICLRRuntimeInfo::GetInterface 方法

将 CLR 加载到当前进程并返回运行时接口指针，如 [ICLRRuntimeHost](iclrruntimehost-interface.md)、 [ICLRStrongName](iclrstrongname-interface.md)和 [IMetaDataDispenserEx](../metadata/imetadatadispenser-interface.md)。  
  
 此方法取代 `CorBindTo` [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md) 部分中的所有 * 函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetInterface(  
[in]  REFCLSID rclsid,  
[in]  REFIID   riid,  
[out, iid_is(riid), retval] LPVOID *ppUnk);  
```  
  
## <a name="parameters"></a>参数  

 `rclsid`  
 中Coclass 的 CLSID 接口。  
  
 `riid`  
 中所请求的接口的 IID `rclsid` 。  
  
 `ppUnk`  
 弄指向查询的接口的指针。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`ppUnk` 为 null。|  
|E_OUTOFMEMORY|没有足够的内存可用来处理请求。|  
|CLR_E_SHIM_LEGACYRUNTIMEALREADYBOUND|已将其他运行时绑定到旧版 CLR 版本2激活策略。|  
  
## <a name="remarks"></a>注解  

 此方法导致加载但不初始化 CLR。  
  
 下表显示了和支持的组合 `rclsid` `riid` 。  
  
|`rclsid`|`riid`|  
|--------------|------------|  
|CLSID_CorMetaDataDispenser|IID_IMetaDataDispenser，IID_IMetaDataDispenserEx|  
|CLSID_CorMetaDataDispenserRuntime|IID_IMetaDataDispenser，IID_IMetaDataDispenserEx|  
|CLSID_CorRuntimeHost|IID_ICorRuntimeHost|  
|CLSID_CLRRuntimeHost|IID_ICLRRuntimeHost|  
|CLSID_TypeNameFactory|IID_ITypeNameFactory|  
|CLSID_CLRDebuggingLegacy|IID_ICorDebug|  
|||  
|CLSID_CLRStrongName|IID_ICLRStrongName|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRRuntimeInfo 接口](iclrruntimeinfo-interface.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
