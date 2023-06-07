---
title: 'GetObjectText 函数 (非托管 API 参考) '
description: GetObjectText 函数以 MOF 语法返回对象的文本呈现方式。
ms.date: 11/06/2017
api_name:
- GetObjectText
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetObjectText
helpviewer_keywords:
- GetObjectText function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 6ad2b29202222d594cc7976a13929002164314a7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718366"
---
# <a name="getobjecttext-function"></a>GetObjectText 函数

返回托管对象格式 (MOF) 语法中的对象的文本呈现方式。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetObjectText (
   [in] int                vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LONG                lFlags,
   [out] BSTR*              pstrObjectText
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`lFlags`  
中通常为0。 如果 `WBEM_FLAG_NO_FLAVORS` 指定 (或 0x1) ，则包括不包含传播或口味信息的限定符。

`pstrObjectText` 弄指向 `null` on 项的指针。 返回时，是一个新分配 `BSTR` 的，其中包含对象的 MOF 语法呈现。  

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的可用内存来完成该操作。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： GetObjectText](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getobjecttext) 方法的调用。

返回的 MOF 文本不包含有关对象的所有信息，但只有足够的信息可供 MOF 编译器重新创建原始对象。 例如，不包含传播的限定符或父类属性。

以下算法用于重新构造方法的参数文本：

1. 参数的标识符值顺序 resequenced。
1. 指定为和的参数 `[in]` `[out]` 合并为单个参数。

`pstrObjectText``null`当调用函数时，必须是指向的指针; 它不能指向方法调用前有效的字符串，因为不会释放指针。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
