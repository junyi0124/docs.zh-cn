---
title: ISymUnmanagedWriter4::GetDebugInfoWithPadding 方法
ms.date: 03/30/2017
ms.assetid: 881e20ca-8131-4bd0-ba41-c2d6391b0fe2
ms.openlocfilehash: 4ac2cccfb17d82d8c62ad7db89161aa794825ae5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678272"
---
# <a name="isymunmanagedwriter4getdebuginfowithpadding-method"></a>ISymUnmanagedWriter4::GetDebugInfoWithPadding 方法

与 [GetDebugInfo 方法](isymunmanagedwriter-getdebuginfo-method.md) 相同，不同之处在于，在终止 null 字符后使用零填充路径字符串，使字符串数据成为固定大小的字符串数据 `MAX_PATH` 。 仅当路径字符串长度本身小于时才会提供填充 `MAX_PATH` 。  
  
 这样可以更轻松地编写不同 PE 文件的工具。  
  
## <a name="syntax"></a>语法  
  
```idl  
HRESULT GetDebugInfoWithPadding(    [in, out] IMAGE_DEBUG_DIRECTORY *pIDD,    [in] DWORD cData,    [out] DWORD *pcData,    [out, size_is(cData), length_is(*pcData)] BYTE data[]);  
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|---------------|-----------------|  
|`pIDD`||  
|`cData`||  
|`pcData`||  
|`data`||  
  
## <a name="return-value"></a>返回值  

 返回 `HRESULT`。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter4 接口](isymunmanagedwriter4-interface.md)
