---
title: IAssemblyCache 接口
ms.date: 03/30/2017
api_name:
- IAssemblyCache
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCache
helpviewer_keywords:
- IAssemblyCache interface [.NET Framework fusion]
ms.assetid: 71ea170f-872d-4fc5-81b6-27da1dec9b19
topic_type:
- apiref
ms.openlocfilehash: df4f0ba018b55202c22cb90b22b927a9c426c4ed
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696851"
---
# <a name="iassemblycache-interface"></a>IAssemblyCache 接口

表示用于合成技术的全局程序集缓存。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateAssemblyCacheItem 方法](iassemblycache-createassemblycacheitem-method.md)|获取对新 [IAssemblyCacheItem](iassemblycacheitem-interface.md)的引用。|  
|[CreateAssemblyScavenger 方法](iassemblycache-createassemblyscavenger-method.md)|保留供合成技术内部使用。|  
|[InstallAssembly 方法](iassemblycache-installassembly-method.md)|在全局程序集缓存中安装指定的程序集。|  
|[QueryAssemblyInfo 方法](iassemblycache-queryassemblyinfo-method.md)|获取有关指定程序集的请求的数据。|  
|[UninstallAssembly 方法](iassemblycache-uninstallassembly-method.md)|从全局程序集缓存中卸载指定的程序集。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
- [全局程序集缓存](../../app-domains/gac.md)
