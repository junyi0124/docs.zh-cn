---
title: QualifierSet_Put 函数（非托管 API 参考）
description: QualifierSet_Put 函数写入指定的限定符及其值。
ms.date: 11/06/2017
api_name:
- QualifierSet_Put
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_Put
helpviewer_keywords:
- QualifierSet_Put function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: a35025c6d16455a51b7b22d822ba77337ddd894a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120237"
---
# <a name="qualifierset_put-function"></a>QualifierSet_Put 函数

写入命名限定符和值。 新限定符覆盖相同名称的以前的值。 如果限定符不存在，则创建它。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT QualifierSet_Put (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LPCWSTR              wszName,
   [in] VARIANT*             pVal,
   [in] LONG                 lFlavor
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)实例的指针。

`wszName`\
中要写入的限定符的名称。

`pVal`\
中指向包含要写入的限定符的有效 `VARIANT` 的指针。 此参数不能为 `null`。

`lFlavor`\
中以下常量之一，定义此限定符所需的限定符风格。 默认值为 `WBEM_FLAVOR_OVERRIDABLE` （0）。

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_FLAVOR_OVERRIDABLE` | 0 | 限定符可以在派生类或实例中重写。 **这是默认值。** |
| `WBEM_FLAVOR_FLAG_PROPAGATE_TO_INSTANCE` | 1 | 限定符传播到实例。 |
| `WBEM_FLAVOR_FLAG_PROPAGATE_TO_DERIVED_CLASS` | 2 | 限定符传播到派生类。 |
| `WBEM_FLAVOR_NOT_OVERRIDABLE` | 0x10 | 不能在派生类或实例中重写限定符。 |
| `WBEM_FLAVOR_AMENDED` | 0x80 | 限定符已本地化。 |

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_E_CANNOT_BE_KEY` | 0x8004101f | 尝试在不能是键的属性上指定**密钥**限定符。 键在对象的类定义中指定，不能基于每个实例进行更改。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
| `WBEM_E_INVALID_QUALIFIER_TYPE` | 0x80041029 | `pVal` 参数不是合法的限定符类型。 |
| `WBEM_E_OVERRIDE_NOT_ALLOWED` | 0x8004101a | 不能对限定符调用 `QualifierSet_Put` 方法，因为所属对象不允许替代。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemQualifierSet：:P](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-put)的工作方式方法的调用。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
