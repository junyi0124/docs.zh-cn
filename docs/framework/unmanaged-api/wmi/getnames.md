---
title: 'GetNames 函数 (非托管 API 参考) '
description: GetNames 函数检索对象的属性的名称。
ms.date: 11/06/2017
api_name:
- GetNames
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetNames
helpviewer_keywords:
- GetNames function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: fd889158e61b86f42d88bcf86eda7d816277e6ac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687653"
---
# <a name="getnames-function"></a>GetNames 函数

检索对象属性的子集或所有名称。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNames (
   [in] int                 vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LPCWSTR             wszQualifierName,
   [in] LONG                lFlags,
   [in] VARIANT*            pQualifierValue,
   [out] SAFEARRAY (BSTR)** pstrNames
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`wszQualifierName`  
中指向有效的指针， `LPCWSTR` 它指定作为筛选器一部分进行操作的限定符名称。 有关详细信息，请参阅[备注](#remarks)部分。 此参数可以为 `null`。

`lFlags`  
中位域的组合。 有关详细信息，请参阅[备注](#remarks)部分。

`pQualifierValue` 中指向 `VARIANT` 初始化为筛选器值的有效结构的指针。 此参数可以为 `null`。

`pstrNames`  
弄 `SAFEARRAY` 包含属性名称的结构。 输入时，此参数必须始终是指向的指针 `null` 。 有关详细信息，请参阅 " [备注](#remarks) " 部分。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 一个或多个参数无效，或者指定的标志和参数组合不正确。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的可用内存来完成该操作。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： GetNames](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getnames) 方法的调用。

命名返回的由标志和参数的组合来控制。 例如，函数可以返回所有属性的名称，或者只返回密钥属性的名称。  主筛选器是在参数中指定的 `lFlags` ，其他参数根据它的不同而不同。

中的标志值 `lFlags` 为位域

可以作为参数传递的标志 `lEnumFlags` 是在 *WbemCli* 头文件中定义的位域，也可以在代码中将它们定义为常量。  可以将每个组中的一个标志与任何其他组中的任何标志组合在一起。 但是，同一组中的标志互相排斥。

| 组1标志 |Value  |说明  |
|---------|---------|---------|
| `WBEM_FLAG_ALWAYS` | 0 | 返回所有属性名称。 `strQualifierName` 和 `pQualifierVal` 均未使用。 |
| `WBEM_FLAG_ONLY_IF_TRUE` | 1 | 仅返回具有参数指定的名称限定符的属性 `strQualifierName` 。 如果使用此标志，则必须指定 `strQualifierName` 。 |
|`WBEM_FLAG_ONLY_IF_FALSE` | 2 |  仅返回不具有参数指定的名称限定符的属性 `strQualifierName` 。 如果使用此标志，则必须指定 `strQualifierName` 。 |
|`WBEM_FLAG_ONLY_IF_IDENTICAL` | 3 | 仅返回具有参数指定的名称限定符的属性，并且其 `wszQualifierName` 值与结构指定的值相同 `pQualifierVal` 。 如果使用此标志，则必须同时指定 `wszQualifierName` 和 `pQualifierValue` 。 |

| 组2标志 |Value  |说明  |
|---------|---------|---------|
|`WBEM_FLAG_KEYS_ONLY` | 0x4 | 仅返回定义键的属性的名称。 |
|`WBEM_FLAG_REFS_ONLY` | 0x8 | 仅返回对象引用的属性名称。 |

| 组3标志 |Value  |说明  |
|---------|---------|---------|
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 仅返回属于派生程度最高的类的属性名称。 排除父类中的属性。 |
| `WBEM_FLAG_PROPAGATED_ONLY` |  0x20 | 仅返回属于父类的属性名称。 |
|`WBEM_FLAG_SYSTEM_ONLY` | 0x30 | 仅返回系统属性的名称。 |
|`WBEM_FLAG_NONSYSTEM_ONLY` | 0x40 | 仅返回非系统属性的名称。 |

如果函数返回，则该函数始终分配一个新的 `SAFEARRAY` `WBEM_S_NO_ERROR` ，并且 `pstrNames` 始终设置为指向该函数。 如果没有属性与指定的筛选器匹配，则返回的数组可以有0个元素。 如果函数返回的值 `WBM_S_NO_ERROR` 不是， `SAFEARRAY` 则不返回新结构。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
