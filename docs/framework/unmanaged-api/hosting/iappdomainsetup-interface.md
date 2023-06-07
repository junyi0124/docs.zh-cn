---
title: IAppDomainSetup 接口
ms.date: 03/30/2017
api_name:
- IAppDomainSetup
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IAppDomainSetup
helpviewer_keywords:
- IAppDomainSetup interface [.NET Framework hosting]
ms.assetid: 1844da85-c031-40bf-bea4-1a3d12a36c8c
topic_type:
- apiref
ms.openlocfilehash: d504101747995557ba526c88de451ebab7b3c556
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95698547"
---
# <a name="iappdomainsetup-interface"></a>IAppDomainSetup 接口

提供属性，这些属性使宿主可以 <xref:System.AppDomain?displayProperty=nameWithType> 在调用 [ICorRuntimeHost：： CreateDomainEx](icorruntimehost-createdomainex-method.md) 方法创建类型之前配置该类型。  
  
## <a name="properties"></a>属性  
  
|properties|说明|  
|--------------|-----------------|  
|<xref:System.AppDomainSetup.ApplicationBase%2A>|获取或设置包含该应用程序的目录的名称。|  
|<xref:System.AppDomainSetup.ApplicationName%2A>|获取或设置应用程序的名称。|  
|<xref:System.AppDomainSetup.CachePath%2A>|获取或设置特定于应用程序的区域的名称，在该区域中对文件进行卷影复制。|  
|<xref:System.AppDomainSetup.ConfigurationFile%2A>|获取或设置应用程序的配置文件的名称。|  
|<xref:System.AppDomainSetup.DynamicBase%2A>|获取或设置用于存储和访问动态生成的文件的目录的名称。|  
|<xref:System.AppDomainSetup.LicenseFile%2A>|获取或设置与此域关联的许可证文件的路径。|  
|<xref:System.AppDomainSetup.PrivateBinPath%2A>|获取或设置与 <xref:System.AppDomainSetup.ApplicationBase%2A> 要探测专用程序集的目录结合在一起的目录列表。|  
|<xref:System.AppDomainSetup.PrivateBinPathProbe%2A>|获取或设置一个字符串值，该值包含或排除 <xref:System.AppDomainSetup.ApplicationBase%2A> 应用程序的搜索路径。|  
|<xref:System.AppDomainSetup.ShadowCopyDirectories%2A>|获取或设置目录的名称，这些目录包含要进行卷影复制的程序集。|  
|<xref:System.AppDomainSetup.ShadowCopyFiles%2A>|获取或设置一个字符串，该字符串指示阴影复制是打开还是关闭。 有效值为 "true" 或 "false"。|  
  
## <a name="remarks"></a>注解  

 `IAppDomainSetup`接口对应于 <xref:System.IAppDomainSetup> 类型实现的托管接口 <xref:System.AppDomainSetup> 。 <xref:System.IAppDomainSetup?displayProperty=nameWithType>有关其属性的详细说明，请参阅。  
  
 `IAppDomainSetup` 表示可以在创建之前添加到实例的程序集绑定信息 <xref:System.AppDomain> 。 例如，主机可以设置 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性以建立根目录，公共语言运行时 (CLR) 探测托管程序集。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.AppDomain>
- <xref:System.AppDomainSetup>
- <xref:System.IAppDomainSetup>
- [承载接口](hosting-interfaces.md)
