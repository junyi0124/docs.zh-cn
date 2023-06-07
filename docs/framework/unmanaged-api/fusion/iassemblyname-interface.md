---
title: IAssemblyName 接口
ms.date: 03/30/2017
api_name:
- IAssemblyName
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyName
helpviewer_keywords:
- IAssemblyName interface [.NET Framework fusion]
ms.assetid: f7f8e605-6b67-4151-936f-f04ecd671d90
topic_type:
- apiref
ms.openlocfilehash: f6feed9f59715f9a2801cd3a2a99a087957d4377
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95715064"
---
# <a name="iassemblyname-interface"></a>IAssemblyName 接口

提供用于描述和处理程序集的唯一标识的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Clone 方法](iassemblyname-clone-method.md)|创建此对象的浅表副本 `IAssemblyName` 。|  
|[Finalize 方法](iassemblyname-finalize-method.md)|允许此 `IAssemblyName` 对象在调用其析构函数之前释放资源并执行其他清理操作。|  
|[GetDisplayName 方法](iassemblyname-getdisplayname-method.md)|获取此对象引用的程序集的可读名称 `IAssemblyName` 。|  
|[GetName 方法](iassemblyname-getname-method.md)|获取此对象引用的程序集的简单、未加密名称 `IAssemblyName` 。|  
|[GetProperty 方法](iassemblyname-getproperty-method.md)|获取一个指针，该指针指向由指定的引用的属性 `PropertyId` 。|  
|[GetVersion 方法](iassemblyname-getversion-method.md)|获取此对象引用的程序集的版本信息 `IAssemblyName` 。|  
|[IsEqual 方法](iassemblyname-isequal-method.md)|`IAssemblyName` `IAssemblyName` 根据指定的比较标志，确定指定的对象是否与此相等。|  
|[SetProperty 方法](iassemblyname-setproperty-method.md)|设置由指定的引用的属性的值 `PropertyId` 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
- [IAssemblyEnum 接口](iassemblyenum-interface.md)
