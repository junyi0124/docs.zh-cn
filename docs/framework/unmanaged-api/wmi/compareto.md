---
title: CompareTo 函数（非托管 API 参考）
description: CompareTo 函数将对象与另一个 WMI 对象进行比较。
ms.date: 11/06/2017
api_name:
- CompareTo
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- CompareTo
helpviewer_keywords:
- CompareTo function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 0d210795016cd2e0179b902a224ca0c62f4ac01f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128699"
---
# <a name="compareto-function"></a>CompareTo 函数

将对象与另一个 Windows 管理对象进行比较。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT CompareTo (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LONG              flags,
   [in] IWbemClassObject* pCompareTo
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例的指针。

`flags`\
中标志的按位组合，用于指定要考虑比较的对象特征。 有关详细信息，请参阅 "[备注](#remarks)" 部分。

`pCompareTo`\
中用于比较的对象。 `pCompareTo` 必须是有效的[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例;不能 `null`。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 发生了未指定的错误。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
| `WBEM_E_UNEXPECTED` | 0x8004101d | 对 `BeginEnumeration` 进行了第二次调用，无需对[`EndEnumeration`](endenumeration.md)进行干预。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
| `WBEM_S_DIFFERENT` | 0x40003 | 对象不同。 |
| `WBEM_S_SAME` | 0 | 根据比较标志，对象是相同的。 |

## <a name="remarks"></a>备注

此函数包装对[IWbemClassObject：： CompareTo](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-compareto)方法的调用。

可以作为 `lEnumFlags` 参数传递的标志在*WbemCli*头文件中定义，也可以在代码中将它们定义为常量。 您可以通过指定下列标志的按位组合来指定比较中涉及的各个特征：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_FLAG_IGNORE_OBJECT_SOURCE` | 2 | 忽略源（服务器及其所属的命名空间）。 |
| `WBEM_FLAG_IGNORE_QUALIFIERS` | 1 | 忽略所有限定符（包括**密钥**和**动态**） |
| `WBEM_FLAG_IGNORE_DEFAULT_VALUES` | 4 | 忽略属性的默认值。 此标志仅适用于对类进行比较。 |
| `WBEM_FLAG_IGNORE_FLAVOR` | 0x20 | 忽略限定符的风格。 此标志仍会考虑限定符，但会忽略风格差别，如传播规则和替代限制。 |
| `WBEM_FLAG_IGNORE_CASE` | 0x10 | 比较字符串值时忽略大小写。 这同时适用于字符串和限定符值。 不管是否设置了此标志，属性和限定符名称的比较始终区分大小写。 |
| `WBEM_FLAG_IGNORE_CLASS` | 0x8 | 假设要比较的对象是同一个类的实例。 因此，此标志仅比较与实例相关的信息。 使用此标志优化性能。 如果对象不属于同一类，则结果是不确定的。 |

也可以按如下所示指定单个复合标志：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|`WBEM_COMPARISON_INCLUDE_ALL` | 0 | 考虑比较中的所有功能。 |

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
