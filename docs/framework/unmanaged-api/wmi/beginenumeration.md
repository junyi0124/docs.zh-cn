---
title: 'Endenumeration 函数 (非托管 API 参考) '
description: Endenumeration 函数将枚举数重置到枚举的开头
ms.date: 11/06/2017
api_name:
- BeginEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- BeginEnumeration
helpviewer_keywords:
- BeginEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 6057526ddbe2efed65f8569e829c35524829e43e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95708213"
---
# <a name="beginenumeration-function"></a>BeginEnumeration 函数

将枚举数重置回枚举的开头。  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT BeginEnumeration (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LONG              lEnumFlags
);
```  

## <a name="parameters"></a>参数

`vFunc`\
中此参数未使用。

`ptr`\
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lEnumFlags`\
中" [备注](#remarks) " 部分中描述的标志或值的按位组合，该组合控制枚举中包含的属性。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 中的标志组合 `lEnumFlags` 无效，或者指定的参数无效。 |
|`WBEM_E_UNEXPECTED` | 0x8004101d | 对的第二次调用 `BeginEnumeration` 没有干预调用 [`EndEnumeration`](endenumeration.md) 。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用于开始新的枚举。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： endenumeration](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 方法的调用。

可以作为参数传递的标志 `lEnumFlags` 在 *WbemCli* 头文件中定义，也可以在代码中将它们定义为常量。  可以将每个组中的一个标志与任何其他组中的任何标志组合在一起。 但是，同一组中的标志互相排斥。

**组 1**

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_FLAG_KEYS_ONLY` | 0x4 | 包括仅构成密钥的属性。 |
|`WBEM_FLAG_REFS_ONLY` | 0x8 | 包括仅为对象引用的属性。 |

**组2**

返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_FLAG_SYSTEM_ONLY` | 0x30 | 仅限枚举到系统属性。 |
|`WBEM_FLAG_NONSYSTEM_ONLY` | 0x40 | 包括本地和传播属性，但不包括枚举中的系统属性。 |

对于类：

返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_FLAG_CLASS_OVERRIDES_ONLY` | 0x100 | 将枚举限制为在类定义中重写的属性。 |
|`WBEM_FLAG_CLASS_LOCAL_AND_OVERRIDES` | 0x100 | 将枚举限制为在当前类定义中重写的属性和在类中定义的新属性。 |
| `WBEM_MASK_CLASS_CONDITION` | 0x300 | 屏蔽 (而不是标记) 应用于 `lEnumFlags` 值以检查是否 `WBEM_FLAG_CLASS_OVERRIDES_ONLY` `WBEM_FLAG_CLASS_LOCAL_AND_OVERRIDES` 设置了或。 |
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 将枚举限制为在类本身中定义或修改的属性。 |
| `WBEM_FLAG_PROPAGATED_ONLY` |  0x20 | 将枚举限制为从基类继承的属性。 |

对于实例：

返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_FLAG_LOCAL_ONLY` | 0x10 | 将枚举限制为在类本身中定义或修改的属性。 |
| `WBEM_FLAG_PROPAGATED_ONLY` |  0x20 | 将枚举限制为从基类继承的属性。 |

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
