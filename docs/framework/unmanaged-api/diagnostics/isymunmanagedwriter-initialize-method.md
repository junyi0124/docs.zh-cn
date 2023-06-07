---
title: ISymUnmanagedWriter::Initialize 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.Initialize
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::Initialize
helpviewer_keywords:
- ISymUnmanagedWriter::Initialize method [.NET Framework debugging]
- Initialize method, ISymUnmanagedWriter interface [.NET Framework debugging]
ms.assetid: e0ebd793-3764-4df0-8f12-0e95f60b9eae
topic_type:
- apiref
ms.openlocfilehash: c702aa32e8c8d6d5c137f7968d1578715102180f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726856"
---
# <a name="isymunmanagedwriterinitialize-method"></a>ISymUnmanagedWriter::Initialize 方法

设置此编写器将与之关联的元数据发射器接口，并设置调试符号将写入的输出文件名。  
  
 此方法只能调用一次，并且必须在任何其他编写器方法之前调用此方法。 某些编写器可能需要文件名。 但是，您始终可以将文件名传递给此方法，而不会对不使用文件名的编写器产生任何负面影响。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Initialize(  
    [in] IUnknown     *emitter,  
    [in] const WCHAR  *filename,  
    [in] IStream      *pIStream,  
    [in] BOOL         fFullBuild);  
```  
  
## <a name="parameters"></a>参数  

 `emitter`  
 中指向元数据发射器接口的指针。  
  
 `filename`  
 中向其中写入调试符号的文件名。 如果为不使用文件名的编写器指定文件名，则忽略此参数。  
  
 `pIStream`  
 中如果已指定，则符号编写器会将符号发送到给定的 <xref:System.Runtime.InteropServices.ComTypes.IStream> （而不是参数中指定的文件） `filename` 。 `pIStream` 参数是可选的。  
  
 `fFullBuild`  
 [in] `true` 如果这是完全重新生成，则为; `false` 如果这是增量编译，则为。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
- [Initialize2 方法](isymunmanagedwriter-initialize2-method.md)
