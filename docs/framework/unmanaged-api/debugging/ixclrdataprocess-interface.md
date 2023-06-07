---
title: IXCLRDataProcess 接口
ms.date: 01/16/2019
api.name:
- IXCLRDataProcess Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess Interface
helpviewer.keywords:
- IXCLRDataProcess Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 376ec2b840bc17c79ed1f27c17a8ddd22c37a0f4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245336"
---
# <a name="ixclrdataprocess-interface"></a>IXCLRDataProcess 接口

提供用于查询有关进程的信息的方法。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a>方法

| 方法                                                                                                                                               | 描述                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [GetRuntimeNameByAddress](ixclrdataprocess-getruntimenamebyaddress-method.md)                     | 获取给定地址的名称。                                                               |
| [GetAppDomainByUniqueId](ixclrdataprocess-getappdomainbyuniqueid-method.md)                       | `AppDomain`按其唯一 id 在进程中获取。                                              |
| [StartEnumModules](ixclrdataprocess-startenummodules-method.md)                                   | 提供枚举进程的模块的句柄。                                        |
| [EnumModule](ixclrdataprocess-enummodule-method.md)                                               | 枚举此进程的模块。                                                         |
| [EndEnumModules](ixclrdataprocess-endenummodules-method.md)                                       | 释放模块枚举期间使用的内部迭代器所使用的资源。               |
| [StartEnumMethodInstancesByAddress](ixclrdataprocess-startenummethodinstancesbyaddress-method.md) | 提供用于枚举 `AppDomain` 从给定地址开始的方法实例的句柄。 |
| [EnumMethodInstanceByAddress](ixclrdataprocess-enummethodinstancebyaddress-method.md)             | 枚举此进程的方法实例（从地址偏移量开始）。                  |
| [EndEnumMethodInstancesByAddress](ixclrdataprocess-endenummethodinstancesbyaddress-method.md)     | 释放实例枚举期间使用的内部迭代器所使用的资源。             |

## <a name="remarks"></a>备注

此接口在运行时中存在，不会通过任何标头或库文件公开。 但是，它是从使用 GUID 派生的 COM 接口， `IUnknown` `5c552ab6-fc09-4cb3-8e36-22fa03c798b7` 该接口可通过常用的 COM 机制获得。

## <a name="requirements"></a>要求

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [调试接口](debugging-interfaces.md)
