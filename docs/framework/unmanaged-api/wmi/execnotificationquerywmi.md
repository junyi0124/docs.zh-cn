---
title: ExecNotificationQueryWmi 函数（非托管 API 参考）
description: ExecNotificationQueryWmi 函数执行查询来接收事件。
ms.date: 11/06/2017
api_name:
- ExecNotificationQueryWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- ExecNotificationQueryWmi
helpviewer_keywords:
- ExecNotificationQueryWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 3d8a7683eef52a5e91bf7aa84d5aa7db7dbdac8d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130440"
---
# <a name="execnotificationquerywmi-function"></a>ExecNotificationQueryWmi 函数

执行查询以接收事件。 调用会立即返回，并且调用方可以在返回的事件发生时轮询返回的枚举器。 释放返回的枚举器将取消查询。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT ExecNotificationQueryWmi (
   [in] BSTR                    strQueryLanguage,
   [in] BSTR                    strQuery,
   [in] long                    lFlags,
   [in] IWbemContext*           pCtx,
   [out] IEnumWbemClassObject** ppEnum,
   [in] DWORD                   authLevel,
   [in] DWORD                   impLevel,
   [in] IWbemServices*          pCurrentNamespace,
   [in] BSTR                    strUser,
   [in] BSTR                    strPassword,
   [in] BSTR                    strAuthority
);
```

## <a name="parameters"></a>参数

`strQueryLanguage`\
中具有 Windows 管理支持的有效查询语言的字符串。 它必须是 "WQL" （WMI 查询语言的首字母缩写）。

`strQuery`\
中查询的文本。 此参数不能为 `null`。

`lFlags`\
中影响此函数的行为的以下两个标志的组合。 这些值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量。

| 返回的常量 | “值”  | 描述  |
|---------|---------|---------|
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | 标志导致半同步调用。 如果未设置此标志，则调用失败。 这是因为事件连续接收，这意味着用户必须轮询返回的枚举器。 无限期地阻止此调用会导致无法做到这一点。 |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | 函数返回一个只进枚举器。 通常，只进枚举器比传统枚举器更快，使用的内存更少，但它们不允许调用[克隆](clone.md)。 |

`pCtx`\
中通常，此值是 `null`的。 否则，它是指向提供请求事件的提供程序可以使用的[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)实例的指针。

`ppEnum`\
弄如果未发生错误，则接收指向枚举器的指针，该枚举器允许调用方检索查询结果集中的实例。 有关详细信息，请参阅 "[备注](#remarks)" 部分。

`authLevel`\
中授权级别。

`impLevel`\
中模拟级别。

`pCurrentNamespace`\
中指向表示当前命名空间的[IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices)对象的指针。

`strUser`\
中用户名。 有关详细信息，请参阅[ConnectServerWmi](connectserverwmi.md)函数。

`strPassword`\
中密码。 有关详细信息，请参阅[ConnectServerWmi](connectserverwmi.md)函数。

`strAuthority`\
中用户的域名。 有关详细信息，请参阅[ConnectServerWmi](connectserverwmi.md)函数。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | 用户无权查看函数可以返回的一个或多个类。 |
| `WBEM_E_FAILED` | 0x80041001 | 发生了未指定的错误。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | 查询指定了一个不存在的类。 |
| `WBEMESS_E_REGISTRATION_TOO_PRECISE` | 0x80042002 | 请求传递事件的精度太多。 必须指定较大的轮询容差。 |
| `WBEMESS_E_REGISTRATION_TOO_BROAD` | 0x80042001 | 查询请求的信息比 Windows 管理提供的信息多。 当事件查询导致请求轮询命名空间中的所有对象时，将返回此 `HRESULT`。 |
| `WBEM_E_INVALID_QUERY` | 0x80041017 | 查询具有语法错误。 |
| `WBEM_E_INVALID_QUERY_TYPE` | 0x80041018 | 不支持请求的查询语言。 |
| `WBEM_E_QUOTA_VIOLATION` | 0x8004106c | 查询太复杂。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用来完成此操作。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI 可能已停止并重新启动。 再次调用[ConnectServerWmi](connectserverwmi.md) 。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 当前进程与 WMI 之间的远程过程调用（RPC）链接失败。 |
| `WBEM_E_UNPARSABLE_QUERY` | 0x80041058 | 无法分析该查询。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemServices：： ExecNotificationQuery](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-execnotificationquery)方法的调用。

函数返回后，调用方定期将返回的 `ppEnum` 对象传递到[下一个](next.md)函数，以查看是否有可用的事件。

可用于 WQL 查询的 `AND` 和 `OR` 关键字的数量有限制。 复杂查询中使用大量的 WQL 关键字可能导致 WMI 以 `HRESULT` 值的形式返回 `WBEM_E_QUOTA_VIOLATION` （或0x8004106c）错误代码。 WQL 关键字的限制取决于查询的复杂程度。

如果函数调用失败，可以通过调用[GetErrorInfo](geterrorinfo.md)函数获取其他错误信息。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
