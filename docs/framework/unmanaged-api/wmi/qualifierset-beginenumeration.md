---
title: QualifierSet_BeginEnumeration 函数（非托管 API 参考）
description: QualifierSet_BeginEnumeration 函数重置对象限定符的枚举器。
ms.date: 11/06/2017
api_name:
- QualifierSet_BeginEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_BeginEnumeration
helpviewer_keywords:
- QualifierSet_BeginEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 79edbd876fc9992f088b9adb159e005c735a72cb
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127329"
---
# <a name="qualifierset_beginenumeration-function"></a>QualifierSet_BeginEnumeration 函数

将对象限定符的枚举器重置到枚举的起始处。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT QualifierSet_BeginEnumeration (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LONG                 lFlags
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)实例的指针。

`lFlags`\
中"[备注](#remarks)" 部分中描述的标志或值的按位组合，用于指定要包括在枚举中的限定符。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `lFlags` 参数无效。 |
|`WBEM_E_UNEXPECTED` | 0x8004101d | 对 `QualifierSet_BeginEnumeration` 进行了第二次调用，无需对[`QualifierSet_EndEnumeration`](qualifierset-endenumeration.md)进行干预。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用于开始新的枚举。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemQualifierSet：： endenumeration](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-beginenumeration)方法的调用。

若要枚举对象的所有限定符，必须在首次调用[QualifierSet_Next](qualifierset-next.md)之前调用此方法。 为给定枚举保证限定符的枚举顺序是固定的。

可以作为 `lEnumFlags` 参数传递的标志在*WbemCli*头文件中定义，也可以在代码中将它们定义为常量。

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|  | 0 | 返回所有限定符的名称。 |
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 仅返回特定于当前属性或对象的限定符的名称。 <br/> 对于属性：仅返回特定于属性的限定符（包括替代），而不返回从类定义传播的限定符。 <br/> 对于实例：仅返回特定于实例的限定符名称。 <br/> 对于类：仅返回特定于要派生的类的限定符。
|`WBEM_FLAG_PROPAGATED_ONLY` | 0x20 | 仅返回从另一个对象传播的限定符的名称。 <br/> 对于属性：返回从类定义传播到此属性的限定符，而不是从属性本身传播到此属性。 <br/> 对于实例：仅返回从类定义传播的限定符。 <br/> 对于类：仅返回从父类继承的那些限定符名称。 |

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
