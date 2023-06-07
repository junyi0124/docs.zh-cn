---
title: ISymUnmanagedReader::ReplaceSymbolStore 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader.ReplaceSymbolStore
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader::ReplaceSymbolStore
helpviewer_keywords:
- ReplaceSymbolStore method [.NET Framework debugging]
- ISymUnmanagedReader::ReplaceSymbolStore method [.NET Framework debugging]
ms.assetid: 43257761-8cb1-4eaf-8fb5-1f3980cb66cd
topic_type:
- apiref
ms.openlocfilehash: 3fa94094ad066496cc8a1fc4dd2ccb0ee16b5aac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675836"
---
# <a name="isymunmanagedreaderreplacesymbolstore-method"></a>ISymUnmanagedReader::ReplaceSymbolStore 方法

用增量符号存储区替换现有的符号存储区。 此方法类似于 [UpdateSymbolStore](isymunmanagedreader-updatesymbolstore-method.md) 方法，不同之处在于给定增量充当完全替换而不是更新。  
  
> [!NOTE]
> 只需指定 `filename` 或 `pIStream` 参数之一，而不能同时指定两者。 如果 `filename` 指定了，则将用该文件中的符号更新符号存储区。 如果 `pIStream` 指定了，则将用中的数据更新存储区 <xref:System.Runtime.InteropServices.ComTypes.IStream> 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReplaceSymbolStore (  
    [in] const WCHAR *filename,  
    [in] IStream *pIStream);  
```  
  
## <a name="parameters"></a>参数  

 `filename`  
 中包含符号存储区的文件的名称。  
  
 `pIStream`  
 中文件流，用作参数的替代项 `filename` 。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedReader 接口](isymunmanagedreader-interface.md)
