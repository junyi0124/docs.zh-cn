---
title: IHostAssemblyStore::ProvideModule 方法
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore.ProvideModule
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore::ProvideModule
helpviewer_keywords:
- IHostAssemblyStore::ProvideModule method [.NET Framework hosting]
- ProvideModule method [.NET Framework hosting]
ms.assetid: f42e3dd0-c88e-4748-b6c0-4c515a633180
topic_type:
- apiref
ms.openlocfilehash: 35805d277774e1de03bb7dee1740a2e1305a97c9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732991"
---
# <a name="ihostassemblystoreprovidemodule-method"></a>IHostAssemblyStore::ProvideModule 方法

解析程序集或链接的 (中的模块，但不解析嵌入) 资源文件。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ProvideModule (  
    [in]  ModuleBindInfo *pBindInfo,  
    [out] DWORD          *pdwModuleId,  
    [out] IStream        **ppStmModuleImage,  
    [out] IStream        **ppStmPDB  
);  
```  
  
## <a name="parameters"></a>参数  

 `pBindInfo`  
 中指向 [ModuleBindInfo](modulebindinfo-structure.md) 实例的指针，该实例描述所请求的模块的 <xref:System.AppDomain> 、程序集和模块名称。  
  
 `pdwModuleId`  
 弄一个指针，指向 `IStream` 包含加载的模块的的唯一标识符。  
  
 `ppStmModuleImage`  
 弄指向对象地址的指针，该 `IStream` 对象包含可移植的可执行文件 (PE) 映像，如果找不到该模块，则为 null。  
  
 `ppStmPDB`  
 弄指向对象地址的指针 `IStream` ，该对象包含请求的模块的程序调试 (PDB) 信息; 如果找不到 .pdb 文件，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`ProvideModule` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|COR_E_FILENOTFOUND (0x80070002) |找不到请求的程序集或链接的资源。|  
|E_NOT_SUFFICIENT_BUFFER|`pdwModuleId` 不够大，无法包含主机希望返回的标识符。|  
  
## <a name="remarks"></a>注解  

 为返回的标识值 `pdwModuleId` 由主机指定。 标识符在进程的生存期内必须是唯一的。 CLR 使用此值作为关联流的唯一标识符。 它将对 ProvideAssembly 和对的调用所返回的值检查每个值 `pAssemblyId` [ProvideAssembly](ihostassemblystore-provideassembly-method.md) `pdwModuleId` `ProvideModule` 。 如果主机为其他主机返回相同的标识符值 `IStream` ，则 CLR 将检查是否已映射该流的内容。 如果是这样，CLR 将加载映像的现有副本，而不是映射一个新副本。 因此，标识符也不得与从返回的程序集标识符重叠 `ProvideAssembly` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
- [IHostAssemblyStore 接口](ihostassemblystore-interface.md)
