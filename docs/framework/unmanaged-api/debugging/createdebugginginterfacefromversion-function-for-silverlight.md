---
title: Silverlight 的 CreateDebuggingInterfaceFromVersion 函数
ms.date: 03/30/2017
f1_keywords:
- CreateDebuggingInterfaceFromVersion
helpviewer_keywords:
- CreateDebuggingInterfaceFromVersion function
- debugging API [Silverlight]
- Silverlight, debugging
ms.assetid: 35c7a18f-133a-4584-bd25-bb338568b0c6
ms.openlocfilehash: f40345b09cae164660711b987f62130518736518
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83208619"
---
# <a name="createdebugginginterfacefromversion-function-for-silverlight"></a>Silverlight 的 CreateDebuggingInterfaceFromVersion 函数

接受从[CreateVersionStringFromModule 函数](createversionstringfrommodule-function.md)返回的公共语言运行时（CLR）版本字符串，并返回相应的调试器接口（通常为[ICorDebug](icordebug-interface.md)）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateDebuggingInterfaceFromVersion (  
    [in]  LPCWSTR      szDebuggeeVersion,  
    [out] IUnknown**   ppCordb,  
);  
```  
  
## <a name="parameters"></a>参数  

 `szDebuggeeVersion`\
 中目标调试对象（由[CreateVersionStringFromModule 函数](createversionstringfrommodule-function.md)返回）中的 CLR 的版本字符串。  
  
 `ppCordb`\
 [out] 指向“COM 对象的指针”的指针 (`IUnknown`)。 此对象将在返回之前强制转换为[ICorDebug](icordebug-interface.md)对象。  
  
## <a name="return-value"></a>返回值

 `S_OK`\
 `ppCordb`引用实现[ICorDebug 接口](icordebug-interface.md)接口的有效对象。  
  
 `E_INVALIDARG`\
 `szDebuggeeVersion` 或 `ppCordb` 为 null。  
  
 `CORDBG_E_DEBUG_COMPONENT_MISSING`\
 找不到 CLR 调试所需的组件。 在与目标 CoreCLR 相同的目录中找不到_mscordbi.dll_或_mscordaccore.dll_ 。  
  
 `CORDBG_E_INCOMPATIBLE_PROTOCOL`\
 mscordbi.dll 或 mscordaccore.dll 与目标 CoreCLR.dll 版本不一致。  
  
 `E_FAIL`（或其他 `E_` 返回代码） \
 无法返回[ICorDebug 接口](icordebug-interface.md)。  
  
## <a name="remarks"></a>备注

 返回的接口提供用于附加到目标进程中的 CLR 和调试 CLR 正在运行的托管代码的功能。  
  
## <a name="requirements"></a>要求

 **平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** dbgshim.dll  
  
 **库：** dbgshim.dll  
  
 **.NET Framework 版本：** 3.5 SP1
