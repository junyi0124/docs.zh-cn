---
title: ICLRRuntimeHost 接口
ms.date: 03/30/2017
api_name:
- ICLRRuntimeHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeHost
helpviewer_keywords:
- ICLRRuntimeHost interface [.NET Framework hosting]
ms.assetid: cb0c5f65-3791-47bc-b833-2f84f4101ba5
topic_type:
- apiref
ms.openlocfilehash: 8d88222215eb31e1c63f3b26079517c4b088e81b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728831"
---
# <a name="iclrruntimehost-interface"></a>ICLRRuntimeHost 接口

提供与 .NET Framework 版本1中提供的 [ICorRuntimeHost](icorruntimehost-interface.md) 接口类似的功能，其中包含以下更改：  
  
- 用于设置宿主控件接口的 [SetHostControl](iclrruntimehost-sethostcontrol-method.md) 方法的添加。  
  
- 省略提供的某些方法 `ICorRuntimeHost` 。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ExecuteApplication 方法](iclrruntimehost-executeapplication-method.md)|在基于清单的 ClickOnce 部署方案中用于指定要在新域中激活的应用程序。|  
|[ExecuteInAppDomain 方法](iclrruntimehost-executeinappdomain-method.md)|指定 <xref:System.AppDomain> 要在其中执行指定的托管代码的。|  
|[ExecuteInDefaultAppDomain 方法](iclrruntimehost-executeindefaultappdomain-method.md)|在指定的程序集中调用指定类型的指定方法。|  
|[GetCLRControl 方法](iclrruntimehost-getclrcontrol-method.md)|获取 [ICLRControl](iclrcontrol-interface.md) 类型的接口指针，宿主可使用该指针自定义公共语言运行时)  (CLR 的各个方面。|  
|[GetCurrentAppDomainId 方法](iclrruntimehost-getcurrentappdomainid-method.md)|获取当前正在执行的的数值标识符 <xref:System.AppDomain> 。|  
|[SetHostControl 方法](iclrruntimehost-sethostcontrol-method.md)|设置宿主控件接口。 `SetHostControl`调用之前必须调用 `Start` 。|  
|[Start 方法](iclrruntimehost-start-method.md)|将 CLR 初始化为一个进程。|  
|[Stop 方法](iclrruntimehost-stop-method.md)|停止由运行时执行的代码。|  
|[UnloadAppDomain 方法](iclrruntimehost-unloadappdomain-method.md)|卸载 <xref:System.AppDomain> 对应于指定数值标识符的。|  
  
## <a name="remarks"></a>注解  

 从 .NET Framework 4 开始，使用 [ICLRMetaHost](iclrmetahost-interface.md) 接口获取指向 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口的指针，然后调用 [ICLRRuntimeInfo：： GetInterface](iclrruntimeinfo-getinterface-method.md) 方法以获取指向的指针 `ICLRRuntimeHost` 。 在 .NET Framework 的早期版本中，主机 `ICLRRuntimeHost` 通过调用 [CorBindToRuntimeEx](corbindtoruntimeex-function.md) 或 [CorBindToCurrentRuntime](corbindtocurrentruntime-function.md)获取指向实例的指针。 若要提供 .NET Framework 版本2.0 中提供的任何技术的实现，必须使用 `ICLRRuntimeHost` 而不是 `ICorRuntimeHost` 。  
  
> [!IMPORTANT]
> 请不要在调用[ExecuteApplication](iclrruntimehost-executeapplication-method.md)方法之前调用[Start](iclrruntimehost-start-method.md)方法来激活基于清单的应用程序。 如果 `Start` 首先调用方法， `ExecuteApplication` 方法调用将失败。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CorBindToCurrentRuntime 函数](corbindtocurrentruntime-function.md)
- [CorBindToRuntimeEx 函数](corbindtoruntimeex-function.md)
- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
- [承载接口](hosting-interfaces.md)
- [CLRRuntimeHost 组件类](clrruntimehost-coclass.md)
