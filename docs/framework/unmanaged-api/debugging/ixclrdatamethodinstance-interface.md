---
title: IXCLRDataMethodInstance 接口
ms.date: 02/01/2019
api.name:
- IXCLRDataMethodInstance Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataMethodInstance Interface
helpviewer.keywords:
- IXCLRDataMethodInstance Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 0b1c982b25af9edea76a038b4314b4bd608f07df
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420885"
---
# <a name="ixclrdatamethodinstance-interface"></a>IXCLRDataMethodInstance 接口

提供用于查询有关方法实例的信息的方法。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a>方法

| 方法                                                                                                                  | 说明                                 |
| ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| [GetILAddressMap](ixclrdatamethodinstance-getiladdressmap-method.md) | 获取用于寻址映射信息的 IL。 |
| [GetRepresentativeEntryAddress](ixclrdatamethodinstance-getrepresentativeentryaddress-method.md) | 获取方法的所有可能入口点的本机编译的最具代表性的入口点地址。 |

## <a name="remarks"></a>备注

此接口在运行时中存在，不会通过任何标头或库文件公开。 但是，它是从使用 GUID 派生的 COM 接口， `IUnknown` `ECD73800-22CA-4b0d-AB55-E9BA7E6318A5` 该接口可通过常用的 COM 机制获得。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [调试接口](debugging-interfaces.md)
