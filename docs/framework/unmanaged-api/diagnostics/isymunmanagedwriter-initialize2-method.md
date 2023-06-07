---
title: ISymUnmanagedWriter::Initialize2 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.Initialize2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::Initialize2
helpviewer_keywords:
- ISymUnmanagedWriter::Initialize2 method [.NET Framework debugging]
- Initialize2 method [.NET Framework debugging]
ms.assetid: 93de56b6-4ae8-4cca-acdc-25a434623509
topic_type:
- apiref
ms.openlocfilehash: 93a96a5da3342f4beff611de1d448dc199dd39dd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687125"
---
# <a name="isymunmanagedwriterinitialize2-method"></a>ISymUnmanagedWriter::Initialize2 方法

设置此编写器将与之关联的元数据发射器接口，并设置调试符号将写入的输出文件名。 此方法还允许您设置程序数据库 (PDB) 文件的最终位置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Initialize2(  
    [in] IUnknown     *emitter,  
    [in] const WCHAR  *tempfilename,  
    [in] IStream      *pIStream,  
    [in] BOOL         fFullBuild,  
    [in] const WCHAR  *finalfilename);  
```  
  
## <a name="parameters"></a>参数  

 `emitter`  
 中指向元数据发射器接口的指针。  
  
 `tempfilename`  
 中指向的指针 `WCHAR` ，该指针包含向其中写入调试符号的文件名。 如果为不使用文件名的编写器指定文件名，则忽略此参数。  
  
 `pIStream`  
 中如果已指定，则符号编写器会将符号发送到给定的 <xref:System.Runtime.InteropServices.ComTypes.IStream> （而不是参数中指定的文件） `filename` 。 `pIStream` 参数是可选的。  
  
 `fFullBuild`  
 [in] `true` 如果这是完全重新生成，则为; `false` 如果这是增量编译，则为。  
  
 `finalfilename`  
 中指向的指针 `WCHAR` ，它是 PDB 文件的最终位置的路径字符串。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
- [Initialize 方法](isymunmanagedwriter-initialize-method.md)
