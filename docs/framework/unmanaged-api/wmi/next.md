---
title: '下一个函数 (非托管 API 引用) '
description: Next 函数检索枚举中的下一个属性。
ms.date: 11/06/2017
api_name:
- Next
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Next
helpviewer_keywords:
- Next function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: c2a7fae32e82caae40a95bfdad10fa78082988ef
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726777"
---
# <a name="next-function"></a>Next 函数

检索枚举中的下一个属性，该属性以对 [endenumeration](beginenumeration.md)的调用开始。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT Next (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LONG              lFlags,
   [out] BSTR*            pstrName,
   [out] VARIANT*         pVal,
   [out] CIMTYPE*         pvtType,
   [out] LONG*            plFlavor
);
```

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lFlags`\
[in] 保留。 此参数必须为0。

`pstrName`\
弄一个新 `BSTR` 的，它包含属性名称。 `null`如果名称不是必需的，则可以将此参数设置为。

`pVal`\
弄 `VARIANT` 用属性的值填充的。 `null`如果值不是必需的，则可以将此参数设置为。 如果函数返回错误代码，则 `VARIANT` `pVal` 不会修改传递到的。

`pvtType`\
弄指向一个变量的指针， `CIMTYPE` 该变量 (`LONG`) 的属性的类型。 此属性的值可以是 `VT_NULL_VARIANT` ，在这种情况下，需要确定属性的实际类型。 此参数还可以为 `null` 。

`plFlavor`\
[out] `null` 或一个值，该值接收有关属性的源的信息。 有关可能的值，请参阅 [备注] 部分。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
| `WBEM_E_UNEXPECTED` | 0x8004101d | 没有对函数的调用 [`BeginEnumeration`](beginenumeration.md) 。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用于开始新的枚举。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 当前进程与 Windows Management 之间的远程过程调用失败。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
| `WBEM_S_NO_MORE_DATA` | 0x40005 | 枚举中没有更多属性。 |

## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： Next](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-next) 方法的调用。

此方法还返回系统属性。

如果属性的基础类型是对象路径、日期或时间或其他特殊类型，则返回的类型不包含足够的信息。 调用方必须检查 `CIMTYPE` 指定属性的，以确定该属性是对象引用、日期、时间还是其他特殊类型。

如果不 `plFlavor` 为 `null` ，则 `LONG` 值接收有关属性的来源的信息，如下所示：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_FLAVOR_ORIGIN_SYSTEM` | 0x40 | 属性为标准系统属性。 |
| `WBEM_FLAVOR_ORIGIN_PROPAGATED` | 0x20 | 对于类：属性是从父类继承的。 <br> 对于实例：从父类继承的属性未被实例修改。  |
| `WBEM_FLAVOR_ORIGIN_LOCAL` | 0 | 对于类：属性属于派生类。 <br> 对于实例：属性由实例进行修改;也就是说，提供了一个值，或者添加或修改了限定符。 |

## <a name="requirements"></a>要求

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
