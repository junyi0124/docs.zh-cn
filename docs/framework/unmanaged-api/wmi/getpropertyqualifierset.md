---
title: GetPropertyQualifierSet 函数（非托管 API 参考）
description: GetPropertyQualifierSet 函数检索为属性设置的限定符。
ms.date: 11/06/2017
api_name:
- GetPropertyQualifierSet
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetPropertyQualifierSet
helpviewer_keywords:
- GetPropertyQualifierSet function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 4133145c7bea1fb3c018d809b9fea3de38270619
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127460"
---
# <a name="getpropertyqualifierset-function"></a>GetPropertyQualifierSet 函数

检索特定属性的限定符集。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT GetPropertyQualifierSet (
   [in] int                 vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LPCWSTR             wszProperty,
   [out] IWbemQualifierSet  **ppQualSet
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例的指针。

`wszMethod`\
中属性名称。 `wszProperty` 必须指向有效 `LPCWSTR`。

`ppQualSet`\
弄接收允许访问属性的限定符的接口指针。 `ppQualSet` 不能为 `null`。 如果发生错误，则不会返回新的对象，并且将指针设置为指向 `null`。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | 指定的方法不存在。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用来完成此操作。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数 `null`。 |
| `WBEM_E_SYSTEM_PROPERTY` | 0x80041030 | 函数尝试获取系统属性的限定符。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemClassObject：： GetPropertyQualifierSet](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getpropertyqualifierset)方法的调用。

仅当当前对象为 CIM 类定义时，才支持调用此函数。 方法操作不可用于指向 CIM 实例的[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)指针。

由于每个方法都可以有自己的限定符，因此， [IWbemQualifierSet 指针](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)使调用方可以添加、编辑或删除这些限定符。

由于系统属性没有限定符，因此如果您尝试获取系统属性的[IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)指针，该函数将返回 `WBEM_E_SYSTEM_PROPERTY`。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
