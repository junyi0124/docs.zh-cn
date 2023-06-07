---
title: DacpMethodDescData 结构
ms.date: 02/01/2019
api.name:
- DacpMethodDescData Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpMethodDescData Structure
helpviewer.keywords:
- DacpMethodDescData Structure [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: dcf01c00a106c131646a16597dca4092a06c5983
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723059"
---
# <a name="dacpmethoddescdata-structure"></a>DacpMethodDescData 结构

定义方法的运行时信息的传输缓冲区。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
struct DacpMethodDescData
{
    int             bHasNativeCode;
    int             bIsDynamic;
    unsigned short  wSlotNumber;
    CLRDATA_ADDRESS NativeCodeAddr;
    CLRDATA_ADDRESS data;
    CLRDATA_ADDRESS MethodDescPtr;
    CLRDATA_ADDRESS nativeCodeInfo;
    CLRDATA_ADDRESS moduleInfo;
    mdToken         MDToken;
    CLRDATA_ADDRESS payloadGC;
    CLRDATA_ADDRESS payloadGC2;
    CLRDATA_ADDRESS managedDynamicMethodObject;
    CLRDATA_ADDRESS requestedIP;
    DacpReJitData   rejitDataCurrent;
    DacpReJitData   rejitDataRequested;
    unsigned long   cJittedRejitVersions;
};
```

## <a name="members"></a>成员

| 成员                       | 说明                                                                                     |
| ---------------------------- | ----------------------------------------------------------------------------------------------- |
| `bHasNativeCode`             | 指示运行时是否具有可用于方法的给定实例化的本机代码。 |
| `bIsDynamic`                 | 指示是否通过轻型代码生成动态生成方法。           |
| `wSlotNumber`                | 方法表中的方法的槽号。                                                   |
| `NativeCodeAddr`             | 方法的初始本机地址。                                                            |
| `data`                       | 指向运行时内部使用的缓冲区的指针。                                             |
| `MethodDescPtr`              | 指向 `MethodDesc` 运行时中的的指针。                                                     |
| `nativeCodeInfo`             | 指向由运行时内部用于跟踪方法的缓冲区的指针。                            |
| `moduleInfo`                 | 指向由运行时内部用于模块信息的缓冲区的指针。                      |
| `MDToken`                    | 与给定方法关联的标记。                                                         |
| `payloadGC`                  | 指向运行时内部使用的垃圾回收缓冲区的指针。                          |
| `payloadGC2`                 | 指向运行时内部使用的垃圾回收缓冲区的指针。                          |
| `managedDynamicMethodObject` | 如果该方法是动态的，则运行时在内部使用此缓冲区进行信息跟踪。     |
| `requestedIP`                | 用于在给定本机代码地址时填充每个请求的结构。                    |
| `rejitDataCurrent`           | 有关该方法的最新检测版本的信息。                                   |
| `rejitDataRequested`         | 请求的本机地址的 Rejit 信息。                                             |
| `cJittedRejitVersions`       | 方法已通过检测 rejitted 的次数。                           |

## <a name="remarks"></a>注解

此结构存在于运行时中，并且不会通过任何标头或库文件公开。 若要使用它，请定义上面指定的结构。

## <a name="requirements"></a>要求

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [调试结构](debugging-structures.md)
- [常见数据类型](../common-data-types-unmanaged-api-reference.md)
