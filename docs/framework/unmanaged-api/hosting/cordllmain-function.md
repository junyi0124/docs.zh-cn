---
title: _CorDllMain 函数
ms.date: 03/30/2017
api_name:
- _CorDllMain
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- _CorDllMain
helpviewer_keywords:
- _CorDllMain function [.NET Framework hosting]
ms.assetid: bc7b51cf-39d3-48ec-a5cb-2f179fbefff8
topic_type:
- apiref
ms.openlocfilehash: 1b3ebcabc66ee7ca29245bb02d958be311bc65fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673691"
---
# <a name="_cordllmain-function"></a>\_CorDllMain 函数

 (CLR) 初始化公共语言运行时，查找 DLL 程序集的 CLR 头中的托管入口点，然后开始执行。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOL STDMETHODCALLTYPE _CorDllMain (  
   [in] HINSTANCE hInst,  
   [in] DWORD     dwReason,  
   [in] LPVOID    lpReserved  
);  
```  
  
## <a name="parameters"></a>参数  

 `hInst`  
 中加载的模块的实例句柄。  
  
 `dwReason`  
 中指示调用 DLL 入口点函数的原因。 此参数可以是下列值之一： DLL \_ PROCESS_ATTACH、dll \_ 线程 \_ 附加、dll \_ 线程 \_ 附加或 dll \_ 进程 \_ 分离。 有关这些值的说明，请参阅 `DllMain` PLATFORM SDK 中的文档。  
  
 `lpReserved`  
 中用.  
  
## <a name="return-value"></a>返回值  

 `true`如果发生错误，则此方法将返回成功 `false` 。  
  
## <a name="remarks"></a>注解  

 此函数由 DLL 程序集的操作系统加载程序调用。 对于可执行程序集，加载程序将调用[ \_ CorExeMain](corexemain-function.md)函数。  
  
 操作系统加载程序将调用此方法，而不考虑 DLL 文件中指定的入口点。  
  
`_CorDllMain`函数由操作系统加载程序直接调用。
  
 有关其他信息，请参阅[ \_ CorValidateImage](corvalidateimage-function.md)主题中的 "备注" 部分。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据全局静态函数](../metadata/metadata-global-static-functions.md)
