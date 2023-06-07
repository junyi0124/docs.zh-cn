---
title: ISymUnmanagedWriter::SetScopeRange 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.SetScopeRange
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::SetScopeRange
helpviewer_keywords:
- SetScopeRange method [.NET Framework debugging]
- ISymUnmanagedWriter::SetScopeRange method [.NET Framework debugging]
ms.assetid: d4d98676-444b-46ca-bfe6-0d827385cd22
topic_type:
- apiref
ms.openlocfilehash: 06dff4847ec3d15f446f1c89219b10eddb8eec4f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683519"
---
# <a name="isymunmanagedwritersetscoperange-method"></a>ISymUnmanagedWriter::SetScopeRange 方法

定义指定词法范围的偏移量范围。 范围将成为新的当前范围，并推送到作用域的堆栈上。 范围必须构成层次结构。 同级不允许重叠。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OpenScope(  
    [in] ULONG32  scopeID,  
    [in] ULONG32  startOffset,  
    [in] ULONG32  endOffset);  
```  
  
## <a name="parameters"></a>参数  

 `scopeId`  
 中作用域的作用域标识符。  
  
 `startOffset`  
 中词法范围中从该方法的开头开始的第一个指令的偏移量（以字节为单位）。  
  
 `endOffset`  
 中词法范围中从该方法的开头开始的最后一个指令的偏移量（以字节为单位）。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="remarks"></a>注解  

 [ISymUnmanagedWriter：： OpenScope](isymunmanagedwriter-openscope-method.md) 返回一个不透明的范围标识符，该标识符可与一起用于 `ISymUnmanagedWriter::SetScopeRange` 定义范围的起始和结束偏移量。 在这种情况下，将忽略传递到 `ISymUnmanagedWriter::OpenScope` 和 [ISymUnmanagedWriter：： CloseScope](isymunmanagedwriter-closescope-method.md) 的偏移量。 范围标识符只在当前方法中有效。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
