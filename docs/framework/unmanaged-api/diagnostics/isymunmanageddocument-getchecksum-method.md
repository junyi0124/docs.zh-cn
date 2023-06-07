---
title: ISymUnmanagedDocument::GetCheckSum 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedDocument.GetCheckSum
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedDocument::GetCheckSum
helpviewer_keywords:
- ISymUnmanagedDocument::GetCheckSum method [.NET Framework debugging]
- GetCheckSum method [.NET Framework debugging]
ms.assetid: 9bc881b3-e2ce-48a7-ad69-17eaaa304120
topic_type:
- apiref
ms.openlocfilehash: 4030da31400b7075952d146e5d6740306863e9ad
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721083"
---
# <a name="isymunmanageddocumentgetchecksum-method"></a>ISymUnmanagedDocument::GetCheckSum 方法

获取校验和。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCheckSum(  
    [in]  ULONG32  cData,  
    [out] ULONG32  *pcData,  
    [out, size_is(cData), length_is(*pcData)] BYTE data[]);  
```  
  
## <a name="parameters"></a>参数  

 `cData`  
 中参数所提供的缓冲区的长度 `data`  
  
 `pcData`  
 弄校验和的大小和长度（以字节为单位）。  
  
 `data`  
 弄接收校验和的缓冲区。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则为错误代码。  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedDocument 接口](isymunmanageddocument-interface.md)
