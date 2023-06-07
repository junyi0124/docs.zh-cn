---
title: ISymUnmanagedWriter::Abort 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.Abort
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::Abort
helpviewer_keywords:
- ISymUnmanagedWriter::Abort method [.NET Framework debugging]
- Abort method, ISymUnmanagedWriter interface [.NET Framework debugging]
ms.assetid: 416b220f-38d4-48e0-bb49-d2faa7366702
topic_type:
- apiref
ms.openlocfilehash: 2136eb32f147b8928e6ac90b99bbdf66804f244d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733264"
---
# <a name="isymunmanagedwriterabort-method"></a>ISymUnmanagedWriter::Abort 方法

关闭符号编写器，而不将符号提交到符号存储区。 在此调用之后，符号编写器将变为无效以便进一步更新。 若要提交符号并关闭符号编写器，请改为使用 [ISymUnmanagedWriter：： close](isymunmanagedwriter-close-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Abort();  
```  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
