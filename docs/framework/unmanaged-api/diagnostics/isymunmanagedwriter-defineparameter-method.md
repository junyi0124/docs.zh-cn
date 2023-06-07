---
title: ISymUnmanagedWriter::DefineParameter 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.DefineParameter
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::DefineParameter
helpviewer_keywords:
- DefineParameter method [.NET Framework debugging]
- ISymUnmanagedWriter::DefineParameter method [.NET Framework debugging]
ms.assetid: a8e3dd32-6a44-4371-9a74-f417b11998c8
topic_type:
- apiref
ms.openlocfilehash: c5e36443295395997303cb94202f534a83d086f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677864"
---
# <a name="isymunmanagedwriterdefineparameter-method"></a>ISymUnmanagedWriter::DefineParameter 方法

在当前方法中定义单个参数。 参数类型从参数的位置获取， (序列) 在方法的签名中。  
  
 如果在给定方法的元数据中定义了参数，则无需使用此方法再次定义这些参数。 符号读取器必须在检查符号存储区之前检查参数的正常元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineParameter(  
    [in] const WCHAR  *name,  
    [in] ULONG32      attributes,  
    [in] ULONG32      sequence,  
    [in] ULONG32      addrKind,  
    [in] ULONG32      addr1,  
    [in] ULONG32      addr2,  
    [in] ULONG32      addr3);  
```  
  
## <a name="parameters"></a>参数  

 `name`  
 中参数名称。  
  
 `attributes`  
 中参数特性。  
  
 `sequence`  
 中参数签名。  
  
 `addrKind`  
 中地址类型。  
  
 `addr1`  
 中参数规范的第一个地址。  
  
 `addr2`  
 中参数规范的第二个地址。  
  
 `addr3`  
 中参数规范的第三个地址。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
