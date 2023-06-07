---
title: ISOSDacInterface 接口
ms.date: 02/01/2019
api.name:
- ISOSDacInterface Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- ISOSDacInterface Interface
helpviewer.keywords:
- ISOSDacInterface Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: c9b4e6e5b36fe38b6c0ea78f6d1848d155008bcc
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420963"
---
# <a name="isosdacinterface-interface"></a>ISOSDacInterface 接口

提供用于访问中的数据的帮助器方法 `SOS` 。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a>方法

| 方法                                                                                                               | 说明                                                                                                                   |
| -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| [GetMethodDescData](isosdacinterface-getmethoddescdata-method.md) | 获取给定 MethodDesc 指针的数据。 |
| [GetMethodDescPtrFromIP](isosdacinterface-getmethoddescptrfromip-method.md) | 检索与包含给定本机指令地址的方法相对应的 MethodDesc 的指针。 |
| [GetModuleData](isosdacinterface-getmoduledata-method.md)| 提取与在给定地址加载的模块对应的数据。 |

## <a name="remarks"></a>备注

此接口在运行时中存在，不会通过任何标头或库文件公开。 但是，它是从使用 GUID 派生的 COM 接口， `IUnknown` `436f00f2-b42a-4b9f-870c-e73db66ae930` 该接口可通过常用的 COM 机制获得。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [调试接口](debugging-interfaces.md)
