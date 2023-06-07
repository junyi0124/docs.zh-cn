---
title: ISymUnmanagedVariable::GetAddressField2 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedVariable.GetAddressField2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedVariable::GetAddressField2
helpviewer_keywords:
- GetAddressField2 method [.NET Framework debugging]
- ISymUnmanagedVariable::GetAddressField2 method [.NET Framework debugging]
ms.assetid: 1f25b294-72b6-4882-a49b-6c9d364b6008
topic_type:
- apiref
ms.openlocfilehash: 858341d5b8b1b3ecbe9dd5bd39a38f9cfd0d08dc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714167"
---
# <a name="isymunmanagedvariablegetaddressfield2-method"></a>ISymUnmanagedVariable::GetAddressField2 方法

获取此变量的第二个地址字段。 其含义取决于地址的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAddressField2(  
    [out, retval] ULONG32* pRetVal);  
```  
  
## <a name="parameters"></a>参数  

 `pRetVal`  
 弄指向的指针 `ULONG32` ，该指针接收第二个地址字段。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedVariable 接口](isymunmanagedvariable-interface.md)
- [GetAddressField1 方法](isymunmanagedvariable-getaddressfield1-method.md)
- [GetAddressField3 方法](isymunmanagedvariable-getaddressfield3-method.md)
- [GetAddressKind 方法](isymunmanagedvariable-getaddresskind-method.md)
