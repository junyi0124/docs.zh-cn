---
title: IHostAssemblyStore::ProvideAssembly 方法
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore.ProvideAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore::ProvideAssembly
helpviewer_keywords:
- ProvideAssembly method [.NET Framework hosting]
- IHostAssemblyStore::ProvideAssembly method [.NET Framework hosting]
ms.assetid: 625c3dd5-a3f0-442c-adde-310dadbb5054
topic_type:
- apiref
ms.openlocfilehash: db65519579104dd01816bb6d7cacaec947f24f53
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680859"
---
# <a name="ihostassemblystoreprovideassembly-method"></a>IHostAssemblyStore::ProvideAssembly 方法

获取对程序集的引用，该程序集未由从[IHostAssemblyManager：： GetNonHostStoreAssemblies](ihostassemblymanager-getnonhoststoreassemblies-method.md)返回的[ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md)引用。 公共语言运行时 (CLR) 对 `ProvideAssembly` 列表中未显示的每个程序集的调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ProvideAssembly (  
    [in]  AssemblyBindInfo *pBindInfo,  
    [out] UINT64           *pAssemblyId,  
    [out] UINT64           *pHostContext,  
    [out] IStream          **ppStmAssemblyImage,  
    [out] IStream          **ppStmPDB  
);  
```  
  
## <a name="parameters"></a>参数  

 `pBindInfo`  
 中指向 [AssemblyBindInfo](assemblybindinfo-structure.md) 实例的指针，主机使用该实例来确定特定的绑定特征，包括是否存在任何版本控制策略以及要绑定到的程序集。  
  
 `pAssemblyId`  
 弄指向此的请求程序集的唯一标识符的指针 `IStream` 。  
  
 `pHostContext`  
 弄指向特定于主机的数据的指针，用于在不需要平台调用的情况下确定请求的程序集的证据。 `pHostContext` 对应于 <xref:System.Reflection.Assembly.HostContext%2A> 托管类的属性 <xref:System.Reflection.Assembly> 。  
  
 `ppStmAssemblyImage`  
 弄指向的地址的指针， `IStream` 该地址包含可移植的可执行文件 (PE) 映像，如果未能找到该程序集，则为 null。  
  
 `ppStmPDB`  
 弄指向 `IStream` 包含程序调试 (pdb) 信息的的地址的指针; 如果找不到 .pdb 文件，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`ProvideAssembly` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|COR_E_FILENOTFOUND (0x80070002) |找不到请求的程序集。|  
|E_NOT_SUFFICIENT_BUFFER|指定的缓冲区大小 `pAssemblyId` 不够大，无法容纳主机需要返回的标识符。|  
  
## <a name="remarks"></a>注解  

 为返回的标识值 `pAssemblyId` 由主机指定。 标识符在进程的生存期内必须是唯一的。 CLR 使用此值作为流的唯一标识符。 它会对照的值检查每个值，以使其他对的 `pAssemblyId` 调用返回 `ProvideAssembly` 。 如果主机为其他主机返回相同的 `pAssemblyId` 值 `IStream` ，则 CLR 将检查是否已映射该流的内容。 如果是这样，则运行时加载映像的现有副本，而不是映射一个新副本。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
- [IHostAssemblyStore 接口](ihostassemblystore-interface.md)
