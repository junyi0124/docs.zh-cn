---
title: ICLRMetaHost::RequestRuntimeLoadedNotification 方法
ms.date: 03/30/2017
api_name:
- ICLRMetaHost.RequestRuntimeLoadedNotification
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost::RequestRuntimeLoadedNotification
helpviewer_keywords:
- RequestRuntimeLoadedNotification method [.NET Framework hosting]
- ICLRMetaHost::RequestRuntimeLoadedNotification method [.NET Framework hosting]
ms.assetid: 0d5ccc4d-0193-41f5-af54-45d7b70d5321
topic_type:
- apiref
ms.openlocfilehash: ac40e203cf7d32c1fe30c9915bac3171139403e0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723276"
---
# <a name="iclrmetahostrequestruntimeloadednotification-method"></a>ICLRMetaHost::RequestRuntimeLoadedNotification 方法

提供一个回调函数，在公共语言运行时 (CLR) 版本第一次加载但尚未启动时，保证调用此函数。 此方法取代了 [LockClrVersion](lockclrversion-function.md) 函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RequestRuntimeLoadedNotification (  
    [in] RuntimeLoadedCallbackFnPtr pCallbackFunction);  
```  
  
## <a name="parameters"></a>参数  

 `pCallbackFunction`  
 中已加载新的运行时调用的回调函数。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`pCallbackFunction` 为 null。|  
  
## <a name="remarks"></a>注解  

 回调的工作方式如下：  
  
- 只有首次加载运行时才会调用回调。  
  
- 对于同一运行时的可重入加载，不调用回调。  
  
- 对于非重入的运行时加载，对回调函数的调用将进行序列化。  
  
 回调函数具有以下原型：  
  
```cpp  
typedef void (__stdcall *RuntimeLoadedCallbackFnPtr)(  
                     ICLRRuntimeInfo *pRuntimeInfo,  
                     CallbackThreadSetFnPtr pfnCallbackThreadSet,  
                     CallbackThreadUnsetFnPtr pfnCallbackThreadUnset);  
```  
  
 回调函数的原型如下所示：  
  
- `pfnCallbackThreadSet`:  
  
    ```cpp  
    typedef HRESULT (__stdcall *CallbackThreadSetFnPtr)();  
    ```  
  
- `pfnCallbackThreadUnset`:  
  
    ```cpp  
    typedef HRESULT (__stdcall *CallbackThreadUnsetFnPtr)();  
    ```  
  
 如果宿主打算加载或导致以重入方式加载另一个运行时，则 `pfnCallbackThreadSet` `pfnCallbackThreadUnset` 必须按以下方式使用回调函数中提供的和参数：  
  
- `pfnCallbackThreadSet` 在尝试执行此类加载之前，必须由可能导致运行时加载的线程调用。  
  
- `pfnCallbackThreadUnset` 当线程不再导致此类运行时加载 (和从初始回调) 返回之前，必须调用。  
  
- `pfnCallbackThreadSet` 和 `pfnCallbackThreadUnset` 都是不可重入的。  
  
> [!NOTE]
> 主机应用程序不得调用 `pfnCallbackThreadSet` `pfnCallbackThreadUnset` 参数范围和参数范围之外的 `pCallbackFunction` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRMetaHost 接口](iclrmetahost-interface.md)
- [承载](index.md)
