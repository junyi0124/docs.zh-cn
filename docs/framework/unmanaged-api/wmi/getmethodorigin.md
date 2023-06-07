---
title: 'GetMethodOrigin 函数 (非托管 API 参考) '
description: GetMethodOrigin 函数确定声明方法的类。
ms.date: 11/06/2017
api_name:
- GetMethodOrigin
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetMethodOrigin
helpviewer_keywords:
- GetMethodOrigin function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 434392ffb4d9124e319bcd9c42fdd340d3fec5b4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722773"
---
# <a name="getmethodorigin-function"></a>GetMethodOrigin 函数

确定声明方法的类。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMethodOrigin (
   [in] int                 vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LPCWSTR             wszMethodName,
   [out] BSTR*              pstrClassName
);
```  

## <a name="parameters"></a>参数

`vFunc`  
中此参数未使用。

`ptr`  
中指向 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) 实例的指针。

`wszMethodName`  
中正在请求其所属类的对象的方法的名称。

`pstrClassName`  
弄接收拥有方法的类的名称。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
|`WBEM_E_NOT_FOUND` | 0x80041002 | 找不到指定的方法。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 一个或多个参数无效。 |
|`WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |
  
## <a name="remarks"></a>注解

此函数包装对 [IWbemClassObject：： GetMethodOrigin](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getmethod) 方法的调用。

由于类可以从一个或多个基类继承方法，因此开发人员通常需要确定定义给定方法的类。

在 `pstrClassName` 调用函数之前，参数必须指向有效的， `BSTR` 因为这是一个 `out` 参数; 在函数返回后，不释放此指针。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
