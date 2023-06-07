---
title: 'ResetSecurity 函数 (非托管 API 参考) '
description: ResetSecurity 函数将模拟标记分配给当前线程。
ms.date: 11/06/2017
api_name:
- ResetSecurity
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- ResetSecurity
helpviewer_keywords:
- ResetSecurity function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 259bef74356f16221f1453dd4086e2fbb26faa83
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721109"
---
# <a name="resetsecurity-function"></a>ResetSecurity 函数

将提供的模拟令牌分配给当前线程。
  
[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ResetSecurity (
   [in] HANDLE token
);
```  

## <a name="parameters"></a>参数

`token`  
中要与当前线程关联的模拟标记。 其值可为 `null`。

## <a name="return-value"></a>返回值

如果该函数成功，则返回值为 `S_OK` (0) 。

如果函数失败，则返回值为非零错误代码。 若要获取扩展的错误信息，请调用 [GetErrorInfo](geterrorinfo.md) 函数。
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** WMINet_Utils .idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
