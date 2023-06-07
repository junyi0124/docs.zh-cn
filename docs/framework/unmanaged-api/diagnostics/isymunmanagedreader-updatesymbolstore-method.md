---
title: ISymUnmanagedReader::UpdateSymbolStore 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader.UpdateSymbolStore
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader::UpdateSymbolStore
helpviewer_keywords:
- UpdateSymbolStore method [.NET Framework debugging]
- ISymUnmanagedReader::UpdateSymbolStore method [.NET Framework debugging]
ms.assetid: 4a17d723-86b9-4f27-bd0d-b70c3259011c
topic_type:
- apiref
ms.openlocfilehash: 6670d985eed4c55550b23d3f4110ee20f3b75661
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675823"
---
# <a name="isymunmanagedreaderupdatesymbolstore-method"></a>ISymUnmanagedReader::UpdateSymbolStore 方法

使用增量符号存储区更新现有的符号存储区。 此方法在 "编辑并继续" 方案中用于更新符号存储区，以便将增量与原始可移植可执行文件 (PE) 文件相匹配。  
  
> [!NOTE]
> 只需指定 `filename` 或 `pIStream` 参数之一，而不能同时指定两者。 如果 `filename` 指定了，则将用该文件中的符号更新符号存储区。 如果 `pIStream` 指定了，则将用中的数据更新存储区 <xref:System.Runtime.InteropServices.ComTypes.IStream> 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT UpdateSymbolStore (  
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
