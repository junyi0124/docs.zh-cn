---
title: " (非托管 API 引用获取函数) "
description: Get 函数检索指定的属性值。
ms.date: 11/06/2017
api_name:
- Get
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Get
helpviewer_keywords:
- Get function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 7cc0c285f79b2791863fce251e4683aa7b55341b
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553683"
---
# <a name="get-function"></a>Get 函数

检索指定的属性值（如果存在）。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT Get (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LPCWSTR           wszName,
   [in] LONG              lFlags,
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

`wszName`\
中属性的名称。

`lFlags`\
[in] 保留。 此参数必须为0。

`pVal`\
弄如果函数成功返回，则包含属性的值 `wszName` 。 为该 `pval` 参数分配了正确的限定符类型和值。

`pvtType`\
弄如果该函数成功返回，则包含一个指示该属性类型的 [CIM 类型常量](/windows/win32/api/wbemcli/ne-wbemcli-cimtype_enumeration) 。 它的值还可以为 `null` 。

`plFlavor`\
弄如果函数成功返回，则接收有关属性的源的信息。 它的值可以是 `null` ，也可以是在 *WbemCli* 头文件中定义的以下 WBEM_FLAVOR_TYPE 常量之一：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_FLAVOR_ORIGIN_SYSTEM` | 0x40 | 属性为标准系统属性。 |
| `WBEM_FLAVOR_ORIGIN_PROPAGATED` | 0x20 | 对于类：属性是从父类继承的。 <br> 对于实例：从父类继承的属性未被实例修改。  |
| `WBEM_FLAVOR_ORIGIN_LOCAL` | 0 | 对于类：属性属于派生类。 <br> 对于实例：属性由实例进行修改;也就是说，提供了一个值，或者添加或修改了限定符。 |

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 一个或多个参数无效。 |
|`WBEM_E_NOT_FOUND` | 0x80041002 | 找不到指定的属性。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的可用内存来完成该操作。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对 [IWbemClassObject：： Get](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-get) 方法的调用。

`Get`函数还可以返回系统属性。

为该 `pVal` 参数分配了正确的限定符类型和值以及 COM [VariantInit](/previous-versions/windows/desktop/api/oleauto/nf-oleauto-variantinit) 函数

## <a name="requirements"></a>要求

 **平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

 **标头：** WMINet_Utils .idl

 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
