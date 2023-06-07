---
title: ICorRuntimeHost 接口
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost
helpviewer_keywords:
- ICorRuntimeHost interface [.NET Framework hosting]
ms.assetid: 4369533d-7834-4497-bc37-bfea0ad737b1
topic_type:
- apiref
ms.openlocfilehash: 9fcb5e189af9f72de7635aad550a5e8ab5522dbd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720615"
---
# <a name="icorruntimehost-interface"></a>ICorRuntimeHost 接口

提供一些方法，这些方法使宿主可以显式启动和停止公共语言运行时 (CLR) ，以创建和配置应用程序域、访问默认域以及枚举在进程中运行的所有域。  
  
 在 .NET Framework 版本2.0 中，此接口由 [ICLRRuntimeHost](iclrruntimehost-interface.md)取代。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CloseEnum 方法](icorruntimehost-closeenum-method.md)|将域枚举器重置回域列表的开头。|  
|[CreateDomain 方法](icorruntimehost-createdomain-method.md)|创建应用程序域。 调用方接收类型为的实例的接口指针 <xref:System._AppDomain> <xref:System.AppDomain?displayProperty=nameWithType> 。|  
|[CreateDomainEx 方法](icorruntimehost-createdomainex-method.md)|创建应用程序域。 此方法允许调用方传递 IAppDomainSetup 实例，以配置返回的实例的附加功能 <xref:System._AppDomain> 。|  
|[CreateDomainSetup 方法](icorruntimehost-createdomainsetup-method.md)|获取实例的类型的接口指针 `IAppDomainSetup` <xref:System.AppDomainSetup> 。 `IAppDomainSetup` 提供用于配置应用程序域在创建之前的各个方面的方法。|  
|[CreateEvidence 方法](icorruntimehost-createevidence-method.md)|获取类型的接口指针 <xref:System.Security.Principal.IIdentity> ，该指针允许主机创建要传递给 [CreateDomain](icorruntimehost-createdomain-method.md) 或 [CreateDomainEx](icorruntimehost-createdomainex-method.md)的安全证据。|  
|[CreateLogicalThreadState 方法](icorruntimehost-createlogicalthreadstate-method.md)|请勿使用。|  
|[CurrentDomain 方法](icorruntimehost-currentdomain-method.md)|获取类型的接口指针 <xref:System._AppDomain> ，该指针表示当前线程上加载的域。|  
|[DeleteLogicalThreadState 方法](icorruntimehost-deletelogicalthreadstate-method.md)|请勿使用。|  
|[EnumDomains 方法](icorruntimehost-enumdomains-method.md)|获取当前进程中的域的枚举数。|  
|[GetConfiguration 方法](icorruntimehost-getconfiguration-method.md)|获取一个对象，该对象允许宿主指定 CLR 的回调配置。|  
|[GetDefaultDomain 方法](icorruntimehost-getdefaultdomain-method.md)|获取类型的接口指针 <xref:System._AppDomain> ，该指针表示当前进程的默认域。|  
|[LocksHeldByLogicalThread 方法](icorruntimehost-locksheldbylogicalthread-method.md)|请勿使用。|  
|[MapFile 方法](icorruntimehost-mapfile-method.md)|将指定的文件映射到内存。 此方法已过时。|  
|[NextDomain 方法](icorruntimehost-nextdomain-method.md)|获取一个接口指针，该指针指向枚举中的下一个域。|  
|[Start 方法](icorruntimehost-start-method.md)|启动 CLR。|  
|[Stop 方法](icorruntimehost-stop-method.md)|停止执行当前进程的运行时中的代码。|  
|[SwitchInLogicalThreadState 方法](icorruntimehost-switchinlogicalthreadstate-method.md)|请勿使用。|  
|[SwitchOutLogicalThreadState 方法](icorruntimehost-switchoutlogicalthreadstate-method.md)|请勿使用。|  
|[UnloadDomain 方法](icorruntimehost-unloaddomain-method.md)|从当前进程中卸载指定的应用程序域。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：** 1.0、1。1  
  
## <a name="see-also"></a>另请参阅

- <xref:System.AppDomain>
- [承载](index.md)
- [ICLRRuntimeHost 接口](iclrruntimehost-interface.md)
- [运行时主机](/previous-versions/dotnet/netframework-4.0/a51xd4ze(v=vs.100))
- [承载接口](hosting-interfaces.md)
- [CorRuntimeHost 组件类](corruntimehost-coclass.md)
