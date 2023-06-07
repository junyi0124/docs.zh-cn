---
title: CreateVersionStringFromModule 函数
ms.date: 03/30/2017
api_name:
- CreateVersionStringFromModule
api_location:
- dbgshim.dll
api_type:
- COM
f1_keywords:
- CreateVersionStringFromModule
helpviewer_keywords:
- debugging API [Silverlight]
- Silverlight, debugging
- CreateVersionStringFromModule function
ms.assetid: 3d2fe9bd-75ef-4364-84a6-da1e1994ac1a
topic_type:
- apiref
ms.openlocfilehash: 1b944034251b34350057866b2a52e63e934d72d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733342"
---
# <a name="createversionstringfrommodule-function"></a>CreateVersionStringFromModule 函数

从目标进程中的公共语言运行时 (CLR) 路径创建版本字符串。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateVersionStringFromModule (  
    [in]  DWORD      pidDebuggee,  
    [in]  LPCWSTR    szModuleName,  
    [out, size_is(cchBuffer),  
          length_is(*pdwLength)] LPWSTR Buffer,  
    [in]  DWORD      cchBuffer,  
    [out] DWORD*     pdwLength  
);  
```  
  
## <a name="parameters"></a>参数  

 `pidDebuggee`  
 [in] 在其中加载目标 CLR 的进程的标识符。  
  
 `szModuleName`  
 [in] 指向进程中加载的目标 CLR 的完整路径或相对路径。  
  
 `pBuffer`  
 [out] 返回用于存储目标 CLR 的版本字符串的缓冲区。  
  
 `cchBuffer`  
 [in] `pBuffer` 的大小。  
  
 `pdwLength`  
 [out] `pBuffer` 返回的版本字符串的长度。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 目标 CLR 的版本字符串已成功返回到 `pBuffer` 中。  
  
 E_INVALIDARG  
 `szModuleName` 为 null，或 `pBuffer` 或 `cchBuffer` 为 null。 `pBuffer` 和 `cchBuffer` 必须都为 null 或非 null。  
  
 HRESULT_FROM_WIN32(ERROR_INSUFFICIENT_BUFFER)  
 `pdwLength` 大于 `cchBuffer`。 如果已为 `pBuffer` 和 `cchBuffer` 都传递了 null，并且已通过使用 `pdwLength` 查询了必要的缓冲区大小，这可能为预期的结果。  
  
 HRESULT_FROM_WIN32(ERROR_MOD_NOT_FOUND)  
 `szModuleName` 不包含指向目标进程中有效 CLR 的路径。  
  
 E_FAIL（或其他 E_ 返回代码）  
 `pidDebuggee` 不引用有效进程，或其他故障。  
  
## <a name="remarks"></a>注解  

 此函数接受由 `pidDebuggee` 标识的 CLR 进程和由 `szModuleName` 指定的字符串路径。 版本字符串返回 `pBuffer` 指向的缓冲区。 此字符串对函数用户是不透明的；这就是说，该版本字符串本身不具有任何实质意义。 它仅用于此函数和 [CreateDebuggingInterfaceFromVersion 函数](createdebugginginterfacefromversion-function-for-silverlight.md)的上下文中。  
  
 应两次调用此函数。 第一次调用此函数时，为 `pBuffer` 和 `cchBuffer` 传递 null。 执行此操作时，`pBuffer` 所需的缓冲区大小将在 `pdwLength` 中返回。 然后可以第二次调用该函数，并将缓冲区传入 `pBuffer` 以及将缓冲区大小传入 `cchBuffer`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** dbgshim.dll  
  
 **库：** dbgshim.dll  
  
 **.NET Framework 版本：** 3.5 SP1
