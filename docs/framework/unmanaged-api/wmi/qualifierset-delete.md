---
title: 'QualifierSet_Delete 函数 (非托管 API 参考) '
description: QualifierSet_Delete 函数按名称删除限定符。
ms.date: 11/06/2017
api_name:
- QualifierSet_Delete
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_Delete
helpviewer_keywords:
- QualifierSet_Delete function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 2000de77903c3dabb43116fa1700b4ed393aeb5a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721148"
---
# <a name="qualifierset_delete-function"></a>QualifierSet_Delete 函数

按名称删除指定限定符。  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT QualifierSet_Delete (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LPCWSTR              wszName
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr` 中指向 [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) 实例的指针。

`wszName` 中要删除的限定符的名称。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `wszName` 参数无效。 |
|`WBEM_E_INVALID_OPERATION` | 0x80041016 | 删除此限定符是非法的。 |
|`WBEM_E_NOT_FOUND` | 0x80041002 | 找不到指定的限定符。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
| `WBEM_S_RESET_TO_DEFAULT` | 0x40002 | 已删除本地替代，并且父对象的原始限定符已恢复范围。 |

## <a name="remarks"></a>注解

此函数包装对 [IWbemQualifierSet：:D e) ](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-delete) 方法的调用。

由于限定符传播规则，特定限定符可能已从另一个对象继承，并且仅在当前类或实例中被重写。 在这种情况下，方法会将 `QualifierSet_Delete` 限定符重置为其原始的继承值。 在这种情况下，函数将返回状态代码 `WBEM_S_RESET_TO_DEFAULT` 。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
