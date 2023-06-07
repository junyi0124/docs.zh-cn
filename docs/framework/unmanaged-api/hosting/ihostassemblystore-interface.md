---
title: IHostAssemblyStore 接口
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore
helpviewer_keywords:
- IHostAssemblyStore interface [.NET Framework hosting]
ms.assetid: cccb650f-abe0-41e2-9fd1-b383788eb1f6
topic_type:
- apiref
ms.openlocfilehash: 4b2fed963d2d0ebec54e5f7a4d95cba26c1bac1f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680932"
---
# <a name="ihostassemblystore-interface"></a>IHostAssemblyStore 接口

提供允许主机独立于公共语言运行时加载程序集和模块 (CLR) 的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ProvideAssembly 方法](ihostassemblystore-provideassembly-method.md)|获取对[IHostAssemblyManager：： GetNonHostStoreAssemblies](ihostassemblymanager-getnonhoststoreassemblies-method.md)调用返回的[ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md)未引用的程序集的引用。|  
|[ProvideModule 方法](ihostassemblystore-providemodule-method.md)|解析程序集内的模块或未嵌入) 资源文件的链接 (。|  
  
## <a name="remarks"></a>注解  

 `IHostAssemblyStore` 提供一种方法，使主机可以根据程序集标识有效地加载程序集。 宿主通过返回直接指向字节的实例来加载程序集 `IStream` 。  
  
 CLR `IHostAssemblyStore` 通过在初始化时调用来确定宿主是否已实现 `IHostAssemblyManager::GetNonHostAssemblyStores` 。 例如，这允许主机控制与用户程序集的绑定，但要依赖于运行时绑定到 .NET Framework 程序集。  
  
> [!NOTE]
> 在提供的实现中 `IHostAssemblyStore` ，主机指定其意图以解析从返回的未引用的所有程序集 `ICLRAssemblyReferenceList` `IHostAssemblyManager::GetNonHostStoreAssemblies` 。  
  
> [!NOTE]
> .NET Framework 版本2.0 不向宿主提供一种方法来加载程序集的本机映像，如 [本机映像生成器 ( # A0) ](../../tools/ngen-exe-native-image-generator.md) 实用工具所提供。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
- [承载接口](hosting-interfaces.md)
