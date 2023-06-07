---
title: QualifierSet_GetNames 函数（非托管 API 参考）
description: QualifierSet_GetNames 函数从对象或属性中检索限定符的名称。
ms.date: 11/06/2017
api_name:
- QualifierSet_GetNames
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_GetNames
helpviewer_keywords:
- QualifierSet_GetNames function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: bd0a67987dd8ffa825114726d066249aed40cf05
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141689"
---
# <a name="qualifierset_getnames-function"></a>QualifierSet_GetNames 函数

检索当前对象或属性中可用的所有限定符或特定限定符的名称。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT QualifierSet_GetNames (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LONG                 lFlags,
   [out] SAFEARRAY (BSTR)**  pstrNames
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)实例的指针。

`lFlags`\
中以下标志或值之一，指定要在枚举中包含的名称。

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|  | 0 | 返回所有限定符的名称。 |
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 仅返回特定于当前属性或对象的限定符的名称。 <br/> 对于属性：仅返回特定于属性的限定符（包括替代），而不返回从类定义传播的限定符。 <br/> 对于实例：仅返回特定于实例的限定符名称。 <br/> 对于类：仅返回特定于要派生的类的限定符。
|`WBEM_FLAG_PROPAGATED_ONLY` | 0x20 | 仅返回从另一个对象传播的限定符的名称。 <br/> 对于属性：返回从类定义传播到此属性的限定符，而不是从属性本身传播到此属性。 <br/> 对于实例：仅返回从类定义传播的限定符。 <br/> 对于类：仅返回从父类继承的那些限定符名称。 |

`pstrNames`\
弄一个新的 `SAFEARRAY`，其中包含请求的名称。 数组可以有0个元素。 如果发生错误，则不会返回新的 `SAFEARRAY`。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用于开始新的枚举。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemQualifierSet：： GetNames](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-getnames)方法的调用。

检索到限定符名称后，可以通过调用[QualifierSet_Get](qualifierset-get.md)函数按名称访问每个限定符。

如果给定的对象具有零限定符，则不是错误的，因此，`pstrNames` on 返回的字符串的数目可以为0，即使该函数返回 `WBEM_S_NO_ERROR`也是如此。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
