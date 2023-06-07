---
title: ISymUnmanagedReader::GetDocumentVersion 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader.GetDocumentVersion
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader::GetDocumentVersion
helpviewer_keywords:
- GetDocumentVersion method [.NET Framework debugging]
- ISymUnmanagedReader::GetDocumentVersion method [.NET Framework debugging]
ms.assetid: a51f1f64-e084-44c5-830c-2222da5a6bbf
topic_type:
- apiref
ms.openlocfilehash: fc38c167b47ea72c7bc7ad81074f9cb1a0d217d8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707561"
---
# <a name="isymunmanagedreadergetdocumentversion-method"></a>ISymUnmanagedReader::GetDocumentVersion 方法

获取指定文档的指定版本。 文档版本从1开始，并在每次使用 [UpdateSymbolStore](isymunmanagedreader-updatesymbolstore-method.md) 方法更新文档时递增。 如果 `pbCurrent` 参数为 `true` ，则这是最新版本的文档。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetDocumentVersion (  
    [in]  ISymUnmanagedDocument *pDoc,  
    [out] int* version,  
    [out] BOOL* pbCurrent);  
```  
  
## <a name="parameters"></a>参数  

 `pDoc`  
 中指定的文档。  
  
 `version`  
 弄指向一个变量的指针，该变量接收指定文档的版本。  
  
 `pbCurrent`  
 弄指向一个变量的指针， `true` 如果这是该文档的最新版本，则为; `false` 如果它不是最新版本，则为。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedReader 接口](isymunmanagedreader-interface.md)
