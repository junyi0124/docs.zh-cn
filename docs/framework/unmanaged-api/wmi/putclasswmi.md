---
title: PutClassWmi 函数（非托管 API 参考）
description: PutClassWmi 函数将创建一个新类或更新现有的类。
ms.date: 11/06/2017
api_name:
- PutClassWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- PutClassWmi
helpviewer_keywords:
- PutClassWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 95a5e1f6339bde9dfe5c5ad9f989fc06e10fa7f8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73101780"
---
# <a name="putclasswmi-function"></a>PutClassWmi 函数

创建新类或更新现有类。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT PutClassWmi (
   [in] IWbemClassObject*    pObject,
   [in] long                 lFlags,
   [in] IWbemContext*        pCtx,
   [out] IWbemCallResult**   ppCallResult
);
```

## <a name="parameters"></a>参数

`pObject`\
中指向有效类定义的指针。 必须用所有必需的属性值正确地对其进行初始化。

`lFlags`\
中影响此函数的行为的标志的组合。 以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 如果已设置，WMI 不会将任何限定符存储在修改后的风格。 <br> 如果未设置，则假定此对象未本地化，且所有限定符都与此实例一起存储。 |
| `WBEM_FLAG_CREATE_OR_UPDATE` | 0 | 如果类不存在，则创建它，如果已存在，则覆盖它。 |
| `WBEM_FLAG_UPDATE_ONLY` | 1 | 更新类。 类必须存在，调用才能成功。 |
| `WBEM_FLAG_CREATE_ONLY` | 2 | 创建类。 如果类已存在，则调用失败。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | 标志导致半同步调用。 |
| `WBEM_FLAG_OWNER_UPDATE` | 0x10000 | 在调用 `PutClassWmi` 指示此类已更改时，推送提供程序必须指定此标志。 |
| `WBEM_FLAG_UPDATE_COMPATIBLE` | 0 | 如果类没有派生类，并且没有该类的实例，则允许更新类。 如果更改只是不重要的限定符（例如描述限定符），则它还允许在所有情况下进行更新。 如果类有实例或更改属于重要限定符，则更新会失败。 |
| `WBEM_FLAG_UPDATE_SAFE_MODE` | 0x20 | 允许更新类（即使有子类），因为更改不会导致与子类冲突。 例如，此标志允许将新的属性添加到基类中，此基类先前未在任何子类中提到。 如果类有实例，则更新会失败。 |
| `WBEM_FLAG_UPDATE_FORCE_MODE` | 0x40 | 存在冲突的子类时强制更新类。 例如，如果类限定符是在子类中定义的，并且基类尝试添加与现有限定符冲突的同一个限定符，则此标志会强制进行更新。 在强制模式下，通过删除子类中的冲突限定符解决了 tis 冲突。 |

`pCtx`\
中通常，此值是 `null`的。 否则，它是指向提供请求的类的提供程序可以使用的[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)实例的指针。

`ppCallResult`\
弄如果 `null`，则不使用此参数。 如果 `lFlags` 包含 `WBEM_FLAG_RETURN_IMMEDIATELY`，则函数立即返回 `WBEM_S_NO_ERROR`。 `ppCallResult` 参数接收指向新[IWbemCallResult](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcallresult)对象的指针。

## <a name="return-value"></a>返回值

此函数返回的以下值是在*WbemCli*头文件中定义的，也可以在代码中将它们定义为常量：

|返回的常量  |“值”  |描述  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | 用户没有创建或修改类的权限。 |
| `WBEM_E_FAILED` | 0x80041001 | 发生了未指定的错误。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | 指定的类无效。 通常，这指示 `pObject` 指定实例对象。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | 参数无效。 |
| `WBEM_E_INVALID OPERATION` | 0x80041016 | 指定的类名无效。 |
| `WBEM_E_CLASS_HAS_CHILDREN` | 0x80041025 | 尝试进行的更改会使子类无效。 |
| `WBEM_E_ALREADY_EXISTS` | 0x80041019 | 指定了 `WBEM_FLAG_CREATE_ONLY` 标志，但该类已存在。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | `WBEM_FLAG_UPDATE_ONLY` 在 `lFlags`中指定，但找不到该类。 |
| `WBEM_E_INCOMPLETE_CLASS` | 0x80041020 | 尚未设置类的必需属性。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 没有足够的内存可用来完成此操作。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI 可能已停止并重新启动。 再次调用[ConnectServerWmi](connectserverwmi.md) 。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 当前进程与 WMI 之间的远程过程调用（RPC）链接失败。 |
| `WBEM_S_NO_ERROR` | 0 | 函数调用成功。  |

## <a name="remarks"></a>备注

此函数包装对[IWbemServices：:P utclass](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-putclass)方法的调用。

用户不能创建名称以下划线字符开头或结尾的类。

如果函数调用失败，可以通过调用[GetErrorInfo](geterrorinfo.md)函数获取其他错误信息。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** WMINet_Utils .idl

**.NET Framework 版本：** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
