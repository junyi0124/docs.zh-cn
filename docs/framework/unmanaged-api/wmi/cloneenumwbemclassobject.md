---
title: 'CloneEnumWbemClassObject 函数 (非托管 API 参考) '
description: CloneEnumWbemClassObject 函数生成枚举器的逻辑副本。
ms.date: 11/06/2017
api_name:
- CloneEnumWbemClassObject
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- CloneEnumWbemClassObject
helpviewer_keywords:
- CloneEnumWbemClassObject function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: fa8a7f436c018e3e083be452d300eb21e17f93f3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95708122"
---
# <a name="cloneenumwbemclassobject-function"></a>CloneEnumWbemClassObject 函数

制作枚举器的逻辑副本，并保留其在枚举中的当前位置。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT CloneEnumWbemClassObject (
   [out] IEnumWbemClassObject**  ppEnum,
   [in] DWORD                    authLevel,
   [in] DWORD                    impLevel,
   [in] IEnumWbemClassObject*    pCurrentEnumWbemClassObject,
   [in] BSTR                     strUser,
   [in] BSTR                     strPassword,
   [in BSTR]                     strAuthority
);
```

## <a name="parameters"></a>参数

`ppEnum`\
弄接收指向新 [IEnumWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-ienumwbemclassobject)的指针。

`authLevel`\
中授权级别。

`impLevel`\
中模拟级别。

`pCurrentEnumWbemClassObject`\
弄指向要克隆的 [IEnumWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-ienumwbemclassobject) 实例的指针。

`strUser`\
中用户名。 有关详细信息，请参阅 [ConnectServerWmi](connectserverwmi.md) 函数。

`strPassword`\
中密码。 有关详细信息，请参阅 [ConnectServerWmi](connectserverwmi.md) 函数。

`strAuthority`\
中用户的域名。 有关详细信息，请参阅 [ConnectServerWmi](connectserverwmi.md) 函数。

## <a name="return-value"></a>返回值

此函数返回的以下值是在 *WbemCli* 头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 出现一般错误。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的可用内存来完成该操作。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 远程过程调用 (RPC) 当前进程和 WMI 之间的链接已失败。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>注解

此函数包装对 [IEnumWbemClassObject：： Clone](/windows/desktop/api/wbemcli/nf-wbemcli-ienumwbemclassobject-clone) 方法的调用。

此方法仅进行 "最大努力" 复制。 由于很多 CIM 对象的动态性质，新枚举器可能不会枚举与源枚举器相同的一组对象。

如果函数调用失败，可以通过调用 [GetErrorInfo](geterrorinfo.md) 函数获取其他错误信息。

## <a name="example"></a>示例

有关示例，请参阅 [IEnumWbemClassObject：： Clone](/windows/desktop/api/wbemcli/nf-wbemcli-ienumwbemclassobject-clone) 方法。

## <a name="requirements"></a>要求

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。

 **标头：** WMINet_Utils .idl

 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
