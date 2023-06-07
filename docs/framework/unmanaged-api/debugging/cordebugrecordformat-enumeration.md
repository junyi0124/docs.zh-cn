---
title: “Cor调试记录格式”枚举
ms.date: 03/30/2017
api_name:
- CorDebugRecordFormat
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: d680c1c0-16ab-4ccc-9444-39cf8e0e05ee
topic_type:
- apiref
ms.openlocfilehash: b3a22d7b32eb258263d373ae91b3fb7fbc9aae99
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696383"
---
# <a name="cordebugrecordformat-enumeration"></a>“Cor调试记录格式”枚举

描述包含本机异常调试事件相关信息的字节数组的数据格式。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugRecordFormat {  
    FORMAT_WINDOWS_EXCEPTIONRECORD32 = 1,  
    FORMAT_WINDOWS_EXCEPTIONRECORD64 = 2,  
} CorDebugRecordFormat;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`FORMAT_WINDOWS_EXCEPTIONRECORD32`|数据为 32 位 Windows 异常记录。|  
|`FORMAT_WINDOWS_EXCEPTIONRECORD64`|数据为 64 位 Windows 异常记录。|  
  
## <a name="remarks"></a>注解  

 枚举的成员 `CorDebugRecordFormat` 将传递给 [DecodeEvent](icordebugprocess6-decodeevent-method.md) 方法，以便在其参数中指示字节数组的格式 `pRecord` 。  
  
> [!NOTE]
> 此枚举仅用于 .NET Native 调试方案。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
