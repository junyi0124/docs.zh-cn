---
title: 'NextMethod 函数 (非托管 API 参考) '
description: NextMethod 函数检索枚举中的下一个方法。
ms.date: 11/06/2017
api_name:
- NextMethod
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- NextMethod
helpviewer_keywords:
- NextMethod function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: a0466aee47b0a6142870640c78b43f49e221ac2b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726764"
---
# <a name="nextmethod-function"></a>NextMethod 函数

检索枚举中的下一个方法，该方法以对 [BeginMethodEnumeration](beginmethodenumeration.md)的调用开始。  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT NextMethod (
   [in] int                 vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LONG                lFlags,
   [out] BSTR*              pName,
   [out] IWbemClassObject** ppInSignature,
   [out] IWbemClassObject** ppOutSignature
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lFlags`  
[in] 保留。 此参数必须为0。

`pName`  
弄指向调用之前的指针 `null` 。 当函数返回时，为 `BSTR` 包含方法名称的新的地址。

`ppSignatureIn`  
弄一个指针，该指针接收指向包含方法的参数的 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 的指针 `in` 。

`ppSignatureOut`  
弄一个指针，该指针接收指向包含方法的参数的 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 的指针 `out` 。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_E_UNEXPECTED` | 0x8004101d | 没有对函数的调用 [`BeginEnumeration`](beginenumeration.md) 。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
| `WBEM_S_NO_MORE_DATA` | 0x40005 | 枚举中没有更多属性。 |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： NextMethod](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-nextmethod) 方法的调用。

调用方通过调用 [BeginMethodEnumeration](beginmethodenumeration.md) 函数开始枚举序列，然后调用 [NextMethod] 函数直到函数返回 `WBEM_S_NO_MORE_DATA` 。 或者，调用方可以通过调用 [EndMethodEnumeration](endmethodenumeration.md)来完成序列。 调用方可以在任何时间通过调用 [EndMethodEnumeration](endmethodenumeration.md) 来提前终止枚举。

## <a name="example"></a>示例

有关 c + + 示例，请参见 [IWbemClassObject：： NextMethod](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-nextmethod) 方法。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
