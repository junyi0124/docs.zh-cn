---
title: ISymUnmanagedDocument::GetSourceRange 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedDocument.GetSourceRange
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedDocument::GetSourceRange
helpviewer_keywords:
- ISymUnmanagedDocument::GetSourceRange method [.NET Framework debugging]
- GetSourceRange method [.NET Framework debugging]
ms.assetid: 20fefee7-1040-41ba-93dc-bd42f68b90c2
topic_type:
- apiref
ms.openlocfilehash: f5d7df60a7b9c728b73fe6592226a8b6734b1e66
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95692145"
---
# <a name="isymunmanageddocumentgetsourcerange-method"></a>ISymUnmanagedDocument::GetSourceRange 方法

将嵌入源的指定范围返回到给定缓冲区中。 缓冲区必须足够大才能容纳源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetSourceRange(  
    [in]  ULONG32  startLine,  
    [in]  ULONG32  startColumn,  
    [in]  ULONG32  endLine,  
    [in]  ULONG32  endColumn,  
    [in]  ULONG32  cSourceBytes,  
    [out] ULONG32  *pcSourceBytes,  
    [out, size_is(cSourceBytes),  
        length_is(*pcSourceBytes)] BYTE source[]);  
```  
  
## <a name="parameters"></a>参数  

 `startLine`  
 中当前文档中的起始行。  
  
 `startColumn`  
 中当前文档中的起始列。  
  
 `endLine`  
 中当前文档中的最后一行。  
  
 `endColumn`  
 中当前文档中的最后一列。  
  
 `cSourceBytes`  
 中源的大小（以字节为单位）。  
  
 `pcSourceBytes`  
 弄指向接收源大小的变量的指针。  
  
 `source`  
 弄源文档的指定范围的大小和长度（以字节为单位）。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则 S_OK。  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedDocument 接口](isymunmanageddocument-interface.md)
