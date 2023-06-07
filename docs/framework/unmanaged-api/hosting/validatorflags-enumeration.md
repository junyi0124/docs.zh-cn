---
title: ValidatorFlags 枚举
ms.date: 03/30/2017
api_name:
- ValidatorFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ValidatorFlags
helpviewer_keywords:
- ValidatorFlags enumeration [.NET Framework hosting]
ms.assetid: a3f5c266-3fcc-4ad1-aaf5-4cdbe26304ad
topic_type:
- apiref
ms.openlocfilehash: 92c430cdb8b46cf75dde9a8395ce713116dc05a5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732848"
---
# <a name="validatorflags-enumeration"></a>ValidatorFlags 枚举

包含一些值，这些值指示应在调用 [ICLRValidator：： Validate](iclrvalidator-validate-method.md) 方法时执行的验证类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
enum ValidatorFlags {  
    VALIDATOR_EXTRA_VERBOSE =       0x00000001,  
    VALIDATOR_SHOW_SOURCE_LINES =   0x00000002,  
    VALIDATOR_CHECK_ILONLY =        0x00000004,  
    VALIDATOR_CHECK_PEFORMAT_ONLY = 0x00000008,  
    VALIDATOR_NOCHECK_PEFORMAT =    0x00000010,  
};  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`VALIDATOR_CHECK_ILONLY`|指定应只验证可执行文件中的 Microsoft 中间语言 (MSIL) 。|  
|`VALIDATOR_CHECK_PEFORMAT_ONLY`|指定只能验证可执行文件的格式。|  
|`VALIDATOR_EXTRA_VERBOSE`|指定应执行和报告的所有类型的验证。|  
|`VALIDATOR_NOCHECK_PEFORMAT`|指定不应验证可执行文件的格式。|  
|`VALIDATOR_SHOW_SOURCE_LINES`|指定验证错误消息应包含引发验证错误的源代码行。 此字段值在 .NET Framework 版本2.0 中无效。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** IValidator，IValidator  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRErrorReportingManager 接口](iclrerrorreportingmanager-interface.md)
- [承载枚举](hosting-enumerations.md)
