---
title: CreateDebuggingInterfaceFromVersion 函数
ms.date: 03/30/2017
api_name:
- CreateDebuggingInterfaceFromVersion
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- CreateDebuggingInterfaceFromVersion
helpviewer_keywords:
- CreateDebuggingInterfaceFromVersion function [.NET Framework hosting]
ms.assetid: a746a849-463c-44f5-a2f0-9e812ed8bcc3
topic_type:
- apiref
ms.openlocfilehash: b68fbc713374642c9f55d49ee51a88c5785cf4b2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727869"
---
# <a name="createdebugginginterfacefromversion-function"></a>CreateDebuggingInterfaceFromVersion 函数

基于指定的版本信息创建 [ICorDebug](../debugging/icordebug-interface.md) 对象。  
  
 此函数在 .NET Framework 4 中已过时。 相反，若要获取公共语言运行时的接口 (CLR) 2.0，请使用 [ICLRRuntimeInfo：： GetInterface](iclrruntimeinfo-getinterface-method.md) 方法并指定类标识符 CLSID_CLRDebuggingLegacy 和接口标识符 IID_ICorDebug。 若要获取 CLR 4 或更高版本的接口，请调用 [CLRCreateInstance](clrcreateinstance-function.md) 函数并指定类标识符 CLSID_CLRDebugging 和接口标识符 IID_ICLRDebugging。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateDebuggingInterfaceFromVersion (  
    [in]  int      iDebuggerVersion,
    [in]  LPCWSTR  szDebuggeeVersion,
    [out] IUnknown **ppCordb  
);  
```  
  
## <a name="parameters"></a>参数  

 `iDebuggerVersion`  
 中调试器所需的的版本 `ICorDebug` 。 有关有效值，请参阅 [CorDebugInterfaceVersion](../debugging/cordebuginterfaceversion-enumeration.md) 枚举。  
  
 `szDebuggeeVersion`  
 中与要调试的应用程序或进程关联的公共语言运行时版本。 有关检索此值的信息，请参阅 [GetVersionFromProcess](getversionfromprocess-function.md) 或 [GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md) 方法。  
  
 `ppCordb`  
 弄接收指向对象的指针的位置 `ICorDebug` 。  
  
## <a name="return-value"></a>返回值  

 除以下值外，此方法还返回 Winerror.h 文件中定义的标准 COM 错误代码。  
  
|返回代码|说明|  
|-----------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_INVALIDARG|`szDebuggeeVersion` 或 `ppCordb` 为 null，或者版本字符串不正确。|  
  
## <a name="remarks"></a>注解  

 `szDebuggeeVersion`参数映射到 MSCorDbi.dll 的相应版本。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
