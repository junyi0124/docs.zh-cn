---
title: 'SpawnDerivedClass 函数 (非托管 API 参考) '
description: SpawnDerivedClass 函数创建派生自对象的新对象。
ms.date: 11/06/2017
api_name:
- SpawnDerivedClass
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- SpawnDerivedClass
helpviewer_keywords:
- SpawnDerivedClass function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 8020dd851b6773e6c76c53892c4b2bc21e4261bb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716507"
---
# <a name="spawnderivedclass-function"></a>SpawnDerivedClass 函数

从指定对象创建新派生的类对象。
  
[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SpawnDerivedClass (
   [in] int                  vFunc,
   [in] IWbemClassObject*    ptr,
   [in] LONG                 lFlags,
   [out] IWbemClassObject**  ppNewClass);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lFlags`  
[in] 保留。 此参数必须为0。

`ppNewClass`  
弄接收指向新类定义对象的指针。 如果发生错误，则不会返回新的对象，也不会 `ppNewClass` 将其保持不变。 它的值不能为 `null` 。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
| `WBEM_E_INVALID_OPERATION` | 0x80041016 | 请求了无效的操作，例如，从实例中生成类。 |
| `WBEM_E_INCOMPLETE_CLASS` | 源类未完全定义或未在 Windows 管理中注册，因此不允许使用新的派生类。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的可用内存来完成该操作。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | `ppNewClass` 为 `null`。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： SpawnDerivedClass](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-clone) 方法的调用。

`ptr` 必须是成为衍生对象的父类的类定义。 返回的对象将成为当前对象的子类。

返回的新对象 `ppNewClass` 将自动成为当前对象的子类。 不能重写此行为。 无法创建子类 (派生类) 的其他方法。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
