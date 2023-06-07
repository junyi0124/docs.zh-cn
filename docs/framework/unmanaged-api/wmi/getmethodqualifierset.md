---
title: GetMethodQualifierSet 函数（非托管 API 参考）
description: GetMethodQualifierSet 函数检索方法的限定符集。
ms.date: 11/06/2017
api_name:
- GetMethodQualifierSet
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetMethodQualifierSet
helpviewer_keywords:
- GetMethodQualifierSet function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 1a36200fd214d013a10ed21c22e1f652de2cbf17
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73102576"
---
# <a name="getmethodqualifierset-function"></a>GetMethodQualifierSet 函数

检索特定方法的限定符集。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT GetMethodQualifierSet (
   [in] int                 vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LPCWSTR             wszMethod,
   [out] IWbemQualifierSet  **ppQualSet
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例的指针。

`wszMethod`\
中方法名称。 `wszMethod` 必须指向有效 `LPCWSTR`。

`ppQualSet`\
弄接收接口指针，该指针允许访问方法的限定符。 `ppQualSet` 不能为 `null`。 如果发生错误，则不会返回新的对象，并且将指针设置为指向 `null`。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|`WBEM_E_NOT_FOUND` | 0x80041002 | 指定的方法不存在。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数 `null`。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemClassObject：： GetMethodQualifierSet](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getmethodqualifierset)方法的调用。

仅当当前对象为 CIM 类定义时，才支持调用此函数。 方法操作不可用于指向 CIM 实例的[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)指针。

由于每个方法都可以有自己的限定符，因此， [IWbemQualifierSet 指针](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)使调用方可以添加、编辑或删除这些限定符。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
