---
title: ICLRMetaHostPolicy::GetRequestedRuntime 方法
ms.date: 03/30/2017
api_name:
- ICLRMetaHostPolicy.GetRequestedRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHostPolicy::GetRequestedRuntime
helpviewer_keywords:
- GetRequestedRuntime method [.NET Framework hosting]
- ICLRMetaHostPolicy::GetRequestedRuntime method [.NET Framework hosting]
ms.assetid: 59ec1832-9cc1-4b5c-983d-03407e51de56
topic_type:
- apiref
ms.openlocfilehash: 37167b7a9aefa6cd9d5e4df043e8bbc1b0514261
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504116"
---
# <a name="iclrmetahostpolicygetrequestedruntime-method"></a>ICLRMetaHostPolicy::GetRequestedRuntime 方法

提供基于宿主策略、托管程序集、版本字符串和配置流的公共语言运行时 (CLR) 的首选版本的接口。 此方法实际上不会加载或激活 CLR，只是返回表示策略结果的[ICLRRuntimeInfo](iclrruntimeinfo-interface.md)接口。 此方法取代了[GetRequestedRuntimeInfo](getrequestedruntimeinfo-function.md)、 [GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md)、 [CorBindToRuntimeHost](corbindtoruntimehost-function.md)、 [CorBindToRuntimeByCfg](corbindtoruntimebycfg-function.md)和[GetCORRequiredVersion](getcorrequiredversion-function.md)方法。

## <a name="syntax"></a>语法

```cpp
HRESULT GetRequestedRuntime(
    [in]  METAHOST_POLICY_FLAGS dwPolicyFlags,
    [in]  LPCWSTR pwzBinary,
    [in]  IStream *pCfgStream,
    [in, out, size_is(*pcchVersion)] LPWSTR pwzVersion,
    [in, out]  DWORD *pcchVersion,
    [out, size_is(*pcchImageVersion)] LPWSTR pwzImageVersion,
    [in, out]  DWORD *pcchImageVersion,
    [out] DWORD *pdwConfigFlags,
    [in]  REFIID  riid
    [out, iid_is(riid), retval] LPVOID *ppRuntime);
```

## <a name="parameters"></a>参数

|名称|说明|
|----------|-----------------|
|`dwPolicyFlags`|[in] 必需。 指定[METAHOST_POLICY_FLAGS](metahost-policy-flags-enumeration.md)枚举的成员，它表示一个绑定策略和任意数量的修饰符。 当前可用的唯一策略是[METAHOST_POLICY_HIGHCOMPAT](metahost-policy-flags-enumeration.md)。<br /><br /> 修饰符包括[METAHOST_POLICY_EMULATE_EXE_LAUNCH](metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_APPLY_UPGRADE_POLICY](metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_SHOW_ERROR_DIALOG](metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_USE_PROCESS_IMAGE_PATH](metahost-policy-flags-enumeration.md)和[METAHOST_POLICY_ENSURE_SKU_SUPPORTED](metahost-policy-flags-enumeration.md)。|
|`pwzBinary`|[in] 可选。 指定程序集文件路径。|
|`pCfgStream`|[in] 可选。 指定配置文件作为 <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType>。|
|`pwzVersion`|[in, out] 可选。 指定或返回要加载的首选 CLR 版本。|
|`pcchVersion`|[in, out] 必需。 输入时指定 `pwzVersion` 的预期的大小，以避免缓冲区溢出。 如果 `pwzVersion` 为 null，则当 `GetRequestedRuntime` 返回时，`pcchVersion` 包含 `pwzVersion` 的预期的大小以允许预分配；否则为 `pcchVersion` 包含写入 `pwzVersion` 的字符数量。|
|`pwzImageVersion`|[out] 可选。 `GetRequestedRuntime`返回时，包含对应于返回的[ICLRRuntimeInfo](iclrruntimeinfo-interface.md)接口的 CLR 版本。|
|`pcchImageVersion`|[in, out] 可选。 输入时指定 `pwzImageVersion` 的预期的大小以避免缓冲区溢出。 如果 `pwzImageVersion` 为 null，则当 `GetRequestedRuntime` 返回时，`pcchImageVersion` 包含 `pwzImageVersion` 的所需大小，以允许预分配。|
|`pdwConfigFlags`|[out] 可选。 如果在 `GetRequestedRuntime` 绑定过程中使用配置文件，则在它返回时， `pdwConfigFlags` 将包含一个[METAHOST_CONFIG_FLAGS](metahost-config-flags-enumeration.md)值，该值指示 [\<startup>](../../configure-apps/file-schema/startup/startup-element.md) 元素是否具有 `useLegacyV2RuntimeActivationPolicy` 属性集和属性的值。 将[METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK](metahost-config-flags-enumeration.md)掩码应用到 `pdwConfigFlags` ，以获取与相关的值 `useLegacyV2RuntimeActivationPolicy` 。|
|`riid`|中为请求的[ICLRRuntimeInfo](iclrruntimeinfo-interface.md)接口指定接口标识符 IID_ICLRRuntimeInfo。|
|`ppRuntime`|弄当 `GetRequestedRuntime` 返回时，包含指向对应的[ICLRRuntimeInfo](iclrruntimeinfo-interface.md)接口的指针。|

## <a name="remarks"></a>注解

如果此方法成功，当且仅当一个或多个以下元素存在于 `<configuration><runtime>` 部分内的配置流中时，该方法具有将其他标志与返回运行时接口的当前默认启动标志结合的副作用：

- `<gcServer enabled="true"/>` 导致 `STARTUP_SERVER_GC` 将被设置。

- `<etwEnable enabled="true"/>` 导致 `STARTUP_ETW` 将被设置。

- `<appDomainResourceMonitoring enabled="true"/>` 导致 `STARTUP_ARM` 将被设置。

产生的默认 `STARTUP_FLAGS` 值是从前述列表设置的值与默认启动标志的按位“或”组合。

## <a name="return-value"></a>返回值

此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。

|HRESULT|说明|
|-------------|-----------------|
|S_OK|该方法已成功完成。|
|E_POINTER|`pwzVersion` 不为 null 且 `pcchVersion` 为 null。<br /><br /> \- 或 -<br /><br /> `pwzImageVersion` 不为 null 且 `pcchImageVersion` 为 null。|
|E_INVALIDARG|`dwPolicyFlags` 未指定 `METAHOST_POLICY_HIGHCOMPAT`。|
|ERROR_INSUFFICIENT_BUFFER|分配给 `pwzVersion` 的内存不足。<br /><br /> \- 或 -<br /><br /> 分配给 `pwzImageVersion` 的内存不足。|
|CLR_E_SHIM_RUNTIMELOAD|`dwPolicyFlags` 包括 METAHOST_POLICY_APPLY_UPGRADE_POLICY，并且 `pwzVersion` 和 `pcchVersion` 均为 null。|

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** MetaHost

**库：** 作为资源包括在 Mscoree.dll 中

**.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]

## <a name="see-also"></a>另请参阅

- [ICLRMetaHostPolicy 接口](iclrmetahostpolicy-interface.md)
- [.NET Framework 4 和 4.5 中添加的 CLR 承载接口](clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
