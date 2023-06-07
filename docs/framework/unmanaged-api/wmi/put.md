---
title: Put 函数（非托管 API 参考）
description: Put 函数为命名属性分配一个新值。
ms.date: 11/06/2017
api_name:
- Put
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Put
helpviewer_keywords:
- Put function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: f1bb8aa09a269e3b8fd23f393d63a275d308a77c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127406"
---
# <a name="put-function"></a>Put 函数

将命名属性设置为新值。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT Put (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LPCWSTR           wszName,
   [in] LONG              lFlags,
   [in] VARIANT*          pVal,
   [in] CIMTYPE           vtType
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)实例的指针。

`wszName`\
中属性的名称。 此参数不能为 `null`。

`lFlags`\
[in] 保留。 此参数必须为0。

`pVal`\
中指向有效 `VARIANT` 的指针，它将成为新的属性值。 如果 `pVal` `null` 或指向类型 `VT_NULL`的 `VARIANT`，则将属性设置为 `null`。

`vtType`\
中`pVal`指向的 `VARIANT` 的类型。 有关详细信息，请参阅 "[备注](#remarks)" 部分。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 一个或多个参数无效。 |
|`WBEM_E_INVALID_PROPERTY_TYPE` | 0x8004102a | 无法识别属性类型。 如果类已经存在，则会在创建类实例时返回此值。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用来完成此操作。 |
| `WBEM_E_TYPE_MISMATCH` | 0x80041005 | 对于实例：指示 `pVal` 指向属性的错误类型的 `VARIANT`。 <br/> 对于类定义：父类中已存在该属性，而新的 COM 类型不同于旧的 COM 类型。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。 |

## <a name="remarks"></a>备注

此函数包装对[IWbemClassObject：:P](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-put)的工作方式方法的调用。

此函数始终使用新的属性值覆盖当前属性值。 如果[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)指向某个类定义，`Put` 将创建或更新该属性值。 当[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)指向 CIM 实例时，`Put` 仅更新属性值;`Put` 无法创建属性值。

只有在创建类的过程中不能留空时，`__CLASS` 系统属性才能写入。 所有其他系统属性都是只读的。

用户不能创建名称以下划线开头或结尾的属性（"_"）。 此为系统类和属性保留。

如果父类中存在由 `Put` 函数设置的属性，则除非属性类型与父类类型不匹配，否则将更改属性的默认值。 如果该属性不存在并且不是类型不匹配，则会创建该属性。

仅当在 CIM 类定义中创建新属性并且 `pVal` `null` 或指向类型 `VT_NULL`的 `VARIANT` 时，才使用 `vtType` 参数。 在这种情况下，`vType` 参数指定属性的 CIM 类型。 在其他所有情况下，`vtType` 必须为0。 如果基础对象是实例，则 `vtType` 也必须为0（即使 `Val` 是 `null`），因为该属性的类型是固定的且无法更改。

## <a name="example"></a>示例

有关示例，请参阅[IWbemClassObject：:P](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-put)工作不上方法。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
