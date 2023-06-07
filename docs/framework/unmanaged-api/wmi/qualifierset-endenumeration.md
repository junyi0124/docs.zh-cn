---
title: 'QualifierSet_EndEnumeration 函数 (非托管 API 参考) '
description: QualifierSet_EndEnumeration 函数终止枚举。
ms.date: 11/06/2017
api_name:
- QualifierSet_EndEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_EndEnumeration
helpviewer_keywords:
- QualifierSet_EndEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 2739003fc9c1f93d379e4a59338cbef7a1a0f135
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726738"
---
# <a name="qualifierset_endenumeration-function"></a>QualifierSet_EndEnumeration 函数

终止通过调用 [QualifierSet_BeginEnumeration](qualifierset-beginenumeration.md) 函数开始的枚举。  

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT QualifierSet_EndEnumeration (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr` 中指向 [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) 实例的指针。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将其定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemQualifierSet：： beginenumeration](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-endenumeration) 方法的调用。

此调用是推荐的，但不是必需的。 它会立即释放与枚举关联的资源。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
**标头：** WMINet_Utils .idl  
  
**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
