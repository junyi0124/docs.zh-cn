---
title: Delete 函数（非托管 API 参考）
description: Delete 函数从 CIM 类定义中删除指定的属性及其所有限定符。
ms.date: 11/06/2017
api_name:
- Delete
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Delete
helpviewer_keywords:
- Delete function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 6b8f287be831702dd31a8335f9b2f6447bcee540
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127665"
---
# <a name="delete-function"></a>Delete 函数

从 CIM 类定义中删除指定的属性及其所有限定符。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT Delete (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LPCWSTR           wszName
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例的指针。

`wszName`\
中要删除的属性的名称。 `wszName` 必须是指向有效 `LPCWSTR`的指针。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 发生了未指定的错误。 |
| `WBEM_E_INVALID_OPERATION` | 0x80041016 | 不能删除该属性。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | `wszName` 无效。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | 指定的属性不存在。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存来完成此操作。 |
| `WBEM_E_PROPAGATED_PROPERTY` | 0x8004101c | 属性继承自基类。 |
| `WBEM_E_SYSTEM_PROPERTY` | | 属性为系统属性。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
| `WBEM_E_RESET_TO_DEFAULT` | 0x80041030 | 函数删除了当前类的替代默认值。 父类中此属性的默认值已重新激活。 |

## <a name="remarks"></a>备注

此函数包装对[IWbemClassObject：:D e)](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-delete)方法的调用。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
