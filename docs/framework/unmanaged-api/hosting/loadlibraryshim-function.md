---
title: LoadLibraryShim 函数
ms.date: 03/30/2017
api_name:
- LoadLibraryShim
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- LoadLibraryShim
helpviewer_keywords:
- LoadLibraryShim function [.NET Framework hosting]
ms.assetid: 30931874-4d0e-4df1-b3d1-e425b50655d1
topic_type:
- apiref
ms.openlocfilehash: d5e9ba0023b6516eb6190f32bc65b2b8b6af79f9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727557"
---
# <a name="loadlibraryshim-function"></a>LoadLibraryShim 函数

加载 .NET Framework 可再发行组件包中包含的 DLL 的指定版本。  
  
 此函数已在 .NET Framework 4 中弃用。 改为使用 [ICLRRuntimeInfo：： LoadLibrary](iclrruntimeinfo-loadlibrary-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT LoadLibraryShim (  
    [in]  LPCWSTR  szDllName,  
    [in]  LPCWSTR  szVersion,  
          LPVOID   pvReserved,  
    [out] HMODULE *phModDll  
);  
```  
  
## <a name="parameters"></a>参数  

 `szDllName`  
 中以零结尾的字符串，表示要从 .NET Framework 库中加载的 DLL 的名称。  
  
 `szVersion`  
 中以零结尾的字符串，表示要加载的 DLL 的版本。 如果 `szVersion` 为 null，则为加载选择的版本是指定 DLL 的最新版本，该版本低于版本4。 也就是说，如果为 null，则忽略等于或大于版本4的所有版本， `szVersion` 如果未安装低于版本4的版本，则 DLL 无法加载。 这是为了确保 .NET Framework 4 的安装不会影响预先存在的应用程序或组件。 请参阅 CLR 团队博客中的 " [进程内 SxS 和迁移快速入门](https://devblogs.microsoft.com/dotnet/in-proc-sxs-and-migration-quick-start/) 条目。  
  
 `pvReserved`  
 留待将来使用。  
  
 `phModDll`  
 弄指向模块的句柄的指针。  
  
## <a name="return-value"></a>返回值  

 除以下值外，此方法还 (COM) 错误代码（如 Winerror.h 中所定义）返回标准组件对象模型。  
  
|返回代码|说明|  
|-----------------|-----------------|  
|S_OK|该方法已成功完成。|  
|CLR_E_SHIM_RUNTIMELOAD|加载 `szDllName` 需要加载公共语言运行时 (clr) ，而 clr 的必要版本无法加载。|  
  
## <a name="remarks"></a>注解  

 此函数用于加载 .NET Framework 可再发行组件包中包含的 Dll。 它不会加载用户生成的 Dll。  
  
> [!NOTE]
> 从 .NET Framework 版本2.0 开始，加载 Fusion.dll 会导致加载 CLR。 这是因为 Fusion.dll 中的函数现在是由运行时提供其实现的包装器。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
