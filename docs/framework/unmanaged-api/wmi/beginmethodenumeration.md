---
title: 'BeginMethodEnumeration 函数 (非托管 API 参考) '
description: BeginMethodEnumeration 函数开始枚举对象的方法
ms.date: 11/06/2017
api_name:
- BeginMethodEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- BeginMethodEnumeration
helpviewer_keywords:
- BeginMethodEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: a72d61a572a0582ed8c03e56a8a9933c5f2775f0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719757"
---
# <a name="beginmethodenumeration-function"></a>BeginMethodEnumeration 函数

开始枚举可用于对象的方法。  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法  
  
```cpp
HRESULT BeginMethodEnumeration (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LONG              lEnumFlags
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lEnumFlags`  
中对于所有方法为零 (0) ，或指定枚举范围的标志。 以下标志是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 将枚举限制为类本身中定义的方法。 |
| `WBEM_FLAG_PROPAGATED_ONLY` |  0x20 | 将枚举限制为从基类继承的属性。 |

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `lEnnumFlags` 不为零，并且不是指定标志之一。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： BeginMethodEnumeration](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-beginmethodenumeration) 方法的调用。

仅当当前对象为类定义时，才支持此方法调用。 指向实例的 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 指针不提供方法操作。 对于给定的 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例，可保证方法的枚举顺序是固定的。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
