---
title: ISymUnmanagedWriter::GetDebugInfo 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.GetDebugInfo
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::GetDebugInfo
helpviewer_keywords:
- ISymUnmanagedWriter::GetDebugInfo method [.NET Framework debugging]
- GetDebugInfo method [.NET Framework debugging]
ms.assetid: dd31c210-6829-45eb-927e-cc53932638b7
topic_type:
- apiref
ms.openlocfilehash: dcab63b603d4a9a8e1430228043d2a5e597bbf4f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678287"
---
# <a name="isymunmanagedwritergetdebuginfo-method"></a>ISymUnmanagedWriter::GetDebugInfo 方法

返回编译器将 (PE 中的调试目录项写入) 文件头所需的信息。 符号编写器填写除和以外的所有 `TimeDateStamp` 字段 `PointerToRawData` 。  (编译器负责正确设置这两个字段。 )   
  
 编译器应调用此方法，将数据 blob 发出到 PE 文件，将 `PointerToRawData` IMAGE_DEBUG_DIRECTORY 中的字段设置为指向发出的数据，并将 IMAGE_DEBUG_DIRECTORY 写入 PE 文件。 编译器还应将该 `TimeDateStamp` 字段设置为与 `TimeDateStamp` 正在生成的 PE 文件的相等。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetDebugInfo(  
    [in, out] IMAGE_DEBUG_DIRECTORY *pIDD,  
    [in]  DWORD cData,  
    [out] DWORD *pcData,  
    [out, size_is(cData),  
        length_is(*pcData)] BYTE data[]);  
```  
  
## <a name="parameters"></a>参数  

 `pIDD`  
 [in，out]指向符号编写器将填写的 IMAGE_DEBUG_DIRECTORY 的指针。  
  
 `cData`  
 中一个 `DWORD` 包含调试数据大小的。  
  
 `pcData`  
 弄指向的指针 `DWORD` ，该指针接收包含调试数据所需的缓冲区大小。  
  
 `data`  
 弄指向缓冲区的指针，该缓冲区足以容纳符号存储区的调试数据。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
