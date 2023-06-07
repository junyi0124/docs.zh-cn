---
title: 'InheritsFrom 函数 (非托管 API 参考) '
description: InheritsFrom 函数确定类或实例是否从特定的父类派生。
ms.date: 11/06/2017
api_name:
- InheritsFrom
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- InheritsFrom
helpviewer_keywords:
- InheritsFrom function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 3cfe3388dc808335e6d3daaf7ec949108e95f52e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726782"
---
# <a name="inheritsfrom-function"></a>InheritsFrom 函数

确定当前类或实例是否派生自指定的父类。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法  
  
```cpp
HRESULT InheritsFrom (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LPCWSTR           wszAncestor
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`wszAncestor`  
中类的名称。 `wszAncestor` 必须指向有效的 `LPCWSTR` 。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_S_NO_ERROR` | 0 | 当前对象继承自 `wszAncestor` 。  |
| `WBEM_S_FALSE` | 1 | 当前的对象不是从继承的 `wszAncestor` 。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | `wszAncestor` 为 `null`。 |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： InheritsFrom](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-inheritsfrom) 方法的调用。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
