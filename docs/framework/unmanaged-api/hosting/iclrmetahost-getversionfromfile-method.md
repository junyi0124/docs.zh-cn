---
title: ICLRMetaHost::GetVersionFromFile 方法
ms.date: 03/30/2017
api_name:
- ICLRMetaHost.GetVersionFromFile
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost::GetVersionFromFile
helpviewer_keywords:
- ICLRMetaHost::GetVersionFromFile method [.NET Framework hosting]
- GetVersionFromFile method [.NET Framework hosting]
ms.assetid: 55bb3eb4-f665-42fc-973c-465567570e82
topic_type:
- apiref
ms.openlocfilehash: f5e0ead41ebd13c259c24fa0c02bcc9a3b33e0fb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689441"
---
# <a name="iclrmetahostgetversionfromfile-method"></a>ICLRMetaHost::GetVersionFromFile 方法

获取程序集的原始 .NET Framework 编译版本 (存储在元) 数据中的的文件路径。 此方法取代了 [GetFileVersion](getfileversion-function.md) 函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetVersionFromFile (  
    [in] LPCWSTR pwzFilePath,  
    [out, size_is(*pcchBuffer)] LPWSTR pwzBuffer,  
    [in, out] DWORD *pcchBuffer);  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzFilePath`  
 中完整的程序集文件路径。  
  
 `pwzbuffer`  
 弄存储在元数据中的 .NET Framework 编译版本，格式为 "v *A*"。*B.*[。*X*] "。 *A*、 *B* 和 *X* 是与主版本、次版本和生成号对应的十进制数字。 此字符串的长度仅限 MAX_PATH。  
  
> [!NOTE]
> 此输出匹配 .NET Framework 版本的目录名称，因为它显示在 C:\Windows\Microsoft.NET\Framework. 下。  
  
 示例值为 "v1.0.3705"、"v 1.1.4322"、"v 2.0.50727" 和 "v4.0"。*X*"，其中 *X* 依赖于安装的生成号。 请注意，"v" 前缀是必需的。  
  
 `pcchBuffer`  
 [in，out] `pwzbuffer` 用于避免缓冲区溢出的的大小。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`pwzbuffer` 或 `pcchBuffer` 为 null。|  
|HRESULT_FROM_WIN32(ERROR_INSUFFICIENT_BUFFER)|缓冲区太小。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRMetaHost 接口](iclrmetahost-interface.md)
- [承载](index.md)
