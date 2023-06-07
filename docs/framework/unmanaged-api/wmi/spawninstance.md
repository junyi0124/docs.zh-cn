---
title: 'SpawnInstance 函数 (非托管 API 参考) '
description: SpawnInstance 函数创建类的新实例。
ms.date: 11/06/2017
api_name:
- SpawnInstance
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- SpawnInstance
helpviewer_keywords:
- SpawnInstance function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 176bc83dd02381af8c2bc3995a37e7fee7c1bebf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732224"
---
# <a name="spawninstance-function"></a>SpawnInstance 函数

创建类的新实例。
  
[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SpawnInstance (
   [in] int                  vFunc,
   [in] IWbemClassObject*    ptr,
   [in] LONG                 lFlags,
   [out] IWbemClassObject**  ppNewInstance);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lFlags`  
[in] 保留。 此参数必须为0。

`ppNewInstance`  
弄接收指向类的新实例的指针。 如果发生错误，则不会返回新的对象，也不会 `ppNewInstance` 将其保持不变。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_E_INCOMPLETE_CLASS` | 0x80041020 | `ptr` 不是有效的类定义，无法生成新的实例。 此方法可能是不完整的，也可能是通过调用 [PutClassWmi](putclasswmi.md)在 Windows 管理中注册的。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的可用内存来完成该操作。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | `ppNewClass` 为 `null`。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： SpawnInstance](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-spawninstance) 方法的调用。

`ptr` 必须是从 Windows Management 获取的类定义。  (请注意，支持从实例生成实例，但返回的实例为空。 ) 然后使用此类定义来创建新的实例。 如果要将实例写入 Windows 管理，则需要调用 [PutInstanceWmi](putinstancewmi.md) 函数。

返回的新对象 `ppNewClass` 将自动成为当前对象的子类。 不能重写此行为。 无法创建子类 (派生类) 的其他方法。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
