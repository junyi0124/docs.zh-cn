---
title: ICLRAssemblyIdentityManager 接口
ms.date: 03/30/2017
api_name:
- ICLRAssemblyIdentityManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRAssemblyIdentityManager
helpviewer_keywords:
- ICLRAssemblyIdentityManager interface [.NET Framework hosting]
ms.assetid: 6a81c6fe-cc22-4062-ae27-d6eeee03a7b9
topic_type:
- apiref
ms.openlocfilehash: 41d049c931091d2cc0b41bd1e9d74b3c15d7878d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679255"
---
# <a name="iclrassemblyidentitymanager-interface"></a>ICLRAssemblyIdentityManager 接口

提供一些方法，这些方法支持宿主与公共语言运行时之间的通信 (CLR) 有关程序集的信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetBindingIdentityFromFile 方法](iclrassemblyidentitymanager-getbindingidentityfromfile-method.md)|获取指定文件路径处的程序集的程序集标识绑定数据。|  
|[GetBindingIdentityFromStream 方法](iclrassemblyidentitymanager-getbindingidentityfromstream-method.md)|获取指定流中的程序集的规范程序集标识数据。|  
|[GetCLRAssemblyReferenceList 方法](iclrassemblyidentitymanager-getclrassemblyreferencelist-method.md)|从提供的部分程序集标识列表中获取 [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) 实例。|  
|[GetProbingAssembliesFromReference 方法](iclrassemblyidentitymanager-getprobingassembliesfromreference-method.md)|获取具有指定标识的程序集所引用的程序集标识的 [ICLRProbingAssemblyEnum](iclrprobingassemblyenum-interface.md) 枚举器。|  
|[GetReferencedAssembliesFromFile 方法](iclrassemblyidentitymanager-getreferencedassembliesfromfile-method.md)|获取一个 [ICLRReferenceAssemblyEnum](iclrreferenceassemblyenum-interface.md) 实例，该实例包含程序集在指定文件路径处引用的程序集的列表。|  
|[GetReferencedAssembliesFromStream 方法](iclrassemblyidentitymanager-getreferencedassembliesfromstream-method.md)|获取一个指向 `ICLRReferenceAssemblyEnum` 对象的指针，该对象包含指定流中的程序集所引用的程序集的程序集标识数据。|  
|[IsStronglyNamed 方法](iclrassemblyidentitymanager-isstronglynamed-method.md)|获取一个值，该值指示指定的程序集是否具有强名称。|  
  
## <a name="remarks"></a>注解  

 用于 `ICLRAssemblyIdentityManager` 获取的实例 `ICLRAssemblyReferenceList` 并枚举程序集标识。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [ICLRProbingAssemblyEnum 接口](iclrprobingassemblyenum-interface.md)
- [承载接口](hosting-interfaces.md)
