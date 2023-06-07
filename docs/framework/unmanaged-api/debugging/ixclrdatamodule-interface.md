---
title: IXCLRDataModule 接口
ms.date: 01/16/2019
api.name:
- IXCLRDataModule Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataModule Interface
helpviewer.keywords:
- IXCLRDataModule Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 3c2bc771c0a131329b9403c99a33ca7b79023771
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420846"
---
# <a name="ixclrdatamodule-interface"></a>IXCLRDataModule 接口

提供用于查询有关已加载模块的信息的方法。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a>方法

| 方法                                                                                                                                | 说明                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [GetMethodDefinitionByToken](ixclrdatamodule-getmethoddefinitionbytoken-method.md) | 获取对应于给定元数据标记的方法定义。 |
| [请求](ixclrdatamodule-request-method.md)                                       | 请求填充模块数据所提供的缓冲区。       |
| [GetVersionId](ixclrdatamodule-getversionid-method.md)                             | 获取模块的版本 ID。                                       |

## <a name="remarks"></a>备注

此接口在运行时中存在，不会通过任何标头或库文件公开。 但是，它是从使用 GUID 派生的 COM 接口， `IUnknown` `88E32849-0A0A-4cb0-9022-7CD2E9E139E2` 该接口可通过常用的 COM 机制获得。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [调试接口](debugging-interfaces.md)
