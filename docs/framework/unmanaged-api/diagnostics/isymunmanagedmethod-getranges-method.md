---
title: ISymUnmanagedMethod::GetRanges 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedMethod.GetRanges
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedMethod::GetRanges
helpviewer_keywords:
- ISymUnmanagedMethod::GetRanges method [.NET Framework debugging]
- GetRanges method [.NET Framework debugging]
ms.assetid: a85283d8-379c-417a-9736-ddeeef9bcf50
topic_type:
- apiref
ms.openlocfilehash: 8ed492b573215736c82ab6c231cc5f2e188ea013
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732146"
---
# <a name="isymunmanagedmethodgetranges-method"></a>ISymUnmanagedMethod::GetRanges 方法

给定文档中的某个位置后，将返回与 Microsoft 中间语言 (MSIL 范围对应的起始和结束偏移量对的数组，) 该位置涵盖在此方法中。 数组是整数数组，格式为 [开始、结束、启动、结束]。 范围对的数目是数组的长度除以2。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRanges(  
    [in]  ISymUnmanagedDocument* document,  
    [in]  ULONG32                line,  
    [in]  ULONG32                column,  
    [in]  ULONG32                cRanges,  
    [out] ULONG32                *pcRanges,  
    [out, size_is(cRanges),  
        length_is(*pcRanges)] ULONG32 ranges[]);  
```  
  
## <a name="parameters"></a>参数  

 `document`  
 中为其请求偏移量的文档。  
  
 `line`  
 中与范围对应的文档行。  
  
 `column`  
 中与范围对应的文档列。  
  
 `cRanges`  
 [in] `ranges` 数组的大小。  
  
 `pcRanges`  
 弄指向的指针 `ULONG32` ，该指针接收包含范围所需的缓冲区大小。  
  
 `ranges`  
 弄指向接收范围的缓冲区的指针。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedMethod 接口](isymunmanagedmethod-interface.md)
