---
title: NukeDownloadedCache 函数
ms.date: 03/30/2017
api_name:
- NukeDownloadedCache
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- DLLExport
f1_keywords:
- NukeDownloadedCache
helpviewer_keywords:
- NukeDownloadedCache function [.NET Framework fusion]
ms.assetid: fac2b1c6-6fa3-4818-805b-b63972024c86
topic_type:
- apiref
ms.openlocfilehash: 5ae0a887d666a150b717d495848c8a411d030a09
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728571"
---
# <a name="nukedownloadedcache-function"></a>NukeDownloadedCache 函数

删除公共语言运行时 (CLR) 下载缓存。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT NukeDownloadedCache();  
```  
  
## <a name="return-value"></a>返回值  

 此方法返回 Winerror.h 中定义的标准 COM 错误代码。  
  
## <a name="remarks"></a>注解  

 CLR 下载缓存是存储从 URL 下载的具有强名称的程序集以便可以重复使用的区域。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **库：** Fusion.dll 和 Mscorwks.dll。 使用 Fusion.dll 而不是 Mscorwks.dll 确保以正确的 .NET Framework 版本为目标。  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CreateHistoryReader 函数](createhistoryreader-function.md)
- [GetHistoryFileDirectory 函数](gethistoryfiledirectory-function.md)
- [合成全局静态函数](fusion-global-static-functions.md)
