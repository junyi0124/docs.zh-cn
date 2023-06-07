---
title: ISymUnmanagedBinder3::GetReaderFromCallback 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedBinder3.GetReaderFromCallback
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedBinder3::GetReaderFromCallback
helpviewer_keywords:
- GetReaderFromCallback method [.NET Framework debugging]
- ISymUnmanagedBinder3::GetReaderFromCallback method [.NET Framework debugging]
ms.assetid: 4ef83bd2-3d8e-499e-8a12-d9d6fd6ced30
topic_type:
- apiref
ms.openlocfilehash: d48c2bdd55e487038048f432c5586d49f393118c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706939"
---
# <a name="isymunmanagedbinder3getreaderfromcallback-method"></a>ISymUnmanagedBinder3::GetReaderFromCallback 方法

允许用户通过回调来实现或提供， `IID_IDiaReadExeAtRVACallback` `IID_IDiaReadExeAtOffsetCallback` 以从内存中获取调试目录信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetReaderFromCallback(  
    [in]  IUnknown     *importer,  
    [in]  const WCHAR  *fileName,  
    [in]  const WCHAR  *searchPath,  
    [in]  ULONG32      searchPolicy,  
    [in]  IUnknown     *callback,  
    [out,retval] ISymUnmanagedReader  **pRetVal);  
```  
  
## <a name="parameters"></a>参数  

 `importer`  
 中指向元数据导入接口的指针。  
  
 `fileName`  
 中指向文件名的指针。  
  
 `searchPath`  
 中指向搜索路径的指针。  
  
 `searchPolicy`  
 中 [CorSymSearchPolicyAttributes](corsymsearchpolicyattributes-enumeration.md) 枚举的一个值，该值指定在搜索符号读取器时要使用的策略。  
  
 `callback`  
 中指向回调函数的指针。  
  
 `pRetVal`  
 弄设置为返回的 [ISymUnmanagedReader](isymunmanagedreader-interface.md) 接口的指针。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym .idl  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedBinder3 接口](isymunmanagedbinder3-interface.md)
