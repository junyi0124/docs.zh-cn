---
title: _CorValidateImage 函数
ms.date: 03/30/2017
api_name:
- _CorValidateImage
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- _CorValidateImage
helpviewer_keywords:
- _CorValidateImage function [.NET Framework hosting]
ms.assetid: 0117e080-05f9-4772-885d-e1847230947c
topic_type:
- apiref
ms.openlocfilehash: 2d49a40610bd0e1a7629594e245bde9eacfcc06d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687972"
---
# <a name="_corvalidateimage-function"></a>_CorValidateImage 函数

验证托管模块映像，并在加载后通知操作系统加载程序。  
  
## <a name="syntax"></a>语法  
  
```cpp  
STDAPI _CorValidateImage (
   [in] PVOID* ImageBase,  
   [in] LPCWSTR FileName  
);  
```  
  
## <a name="parameters"></a>参数  

 `ImageBase`  
 中一个指针，指向要作为托管代码验证的图像的起始位置。 必须已将该映像加载到内存中。  
  
 `FileName`  
 中图像的文件名。  
  
## <a name="return-value"></a>返回值  

 此函数返回标准值 `E_INVALIDARG` 、、 `E_OUTOFMEMORY` `E_UNEXPECTED` 和，以及 `E_FAIL` 以下值。  
  
|返回值|说明|  
|------------------|-----------------|  
|`STATUS_INVALID_IMAGE_FORMAT`|图像无效。 此值具有 HRESULT 0xC000007BL。|  
|`STATUS_SUCCESS`|图像有效。 此值具有 HRESULT 0x00000000L。|  
  
## <a name="remarks"></a>注解  

 在 Windows XP 和更高版本中，操作系统加载程序通过检查公共对象文件格式 (COFF) 标头的 COM 描述符目录位来检查托管模块。 设置位表示托管模块。 如果加载程序检测到托管模块，它将加载 MsCorEE.dll 和调用 `_CorValidateImage` ，这会执行以下操作：  
  
- 确认映像是有效的托管模块。  
  
- 将映像中的入口点更改为公共语言运行时中的入口点 (CLR) 。  
  
- 对于64位版本的 Windows，通过将其从 PE32 转换为 PE32 + 格式修改内存中的映像。  
  
- 加载托管模块映像时返回到加载程序。  
  
 对于可执行映像，操作系统加载器会调用 [_CorExeMain](corexemain-function.md) 函数，而不考虑可执行文件中指定的入口点。 对于 DLL 程序集映像，加载程序将调用 [_CorDllMain](cordllmain-function.md) 函数。  
  
 `_CorExeMain` 或 `_CorDllMain` 执行以下操作：  
  
- 初始化 CLR。  
  
- 定位程序集的 CLR 头中的托管入口点。  
  
- 开始执行。  
  
 卸载托管模块映像时，加载程序将调用 [_CorImageUnloading](corimageunloading-function.md) 函数。 但是，此函数不执行任何操作;它仅返回。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据全局静态函数](../metadata/metadata-global-static-functions.md)
