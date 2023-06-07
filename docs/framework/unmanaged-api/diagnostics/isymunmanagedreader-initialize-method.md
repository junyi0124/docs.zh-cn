---
title: ISymUnmanagedReader::Initialize 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader.Initialize
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader::Initialize
helpviewer_keywords:
- ISymUnmanagedReader::Initialize method [.NET Framework debugging]
- Initialize method, ISymUnmanagedReader interface [.NET Framework debugging]
ms.assetid: 8f0dd2fe-7df7-464e-91f4-5518c586bb5f
topic_type:
- apiref
ms.openlocfilehash: 6193d91c8cbe0efa7cd68b97b9262acf72c9ea0b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675875"
---
# <a name="isymunmanagedreaderinitialize-method"></a>ISymUnmanagedReader::Initialize 方法

用此读取器将与之关联的元数据导入程序接口以及模块的文件名初始化符号读取器。  
  
> [!NOTE]
> 此方法只能调用一次，并且必须在任何其他读取器方法之前调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Initialize (  
    [in]  IUnknown     *importer,  
    [in]  const WCHAR  *filename,  
    [in]  const WCHAR  *searchPath,  
    [in]  IStream      *pIStream);  
```  
  
## <a name="parameters"></a>参数  

 `importer`  
 中此读取器将与之关联的元数据导入程序接口。  
  
 `filename`  
 中模块的文件名。 可以改为使用 `pIStream` 参数。  
  
 `searchPath`  
 中要搜索的路径。 此参数是可选的。  
  
 `pIStream`  
 中文件流，用作 filename 参数的替代项。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="remarks"></a>注解  

 只需指定 `filename` 或 `pIStream` 参数之一，而不能同时指定两者。 `searchPath` 参数是可选的。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedReader 接口](isymunmanagedreader-interface.md)
