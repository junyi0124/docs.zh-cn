---
title: ISymUnmanagedVariable::GetAddressField3 方法
ms.date: 03/30/2017
api_name:
- ISymUnmanagedVariable.GetAddressField3
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedVariable::GetAddressField3
helpviewer_keywords:
- ISymUnmanagedVariable::GetAddressField3 method [.NET Framework debugging]
- GetAddressField3 method [.NET Framework debugging]
ms.assetid: 4d834721-ad8d-439d-b356-c6b4aef022fc
topic_type:
- apiref
ms.openlocfilehash: 13746c4ac6322a401e547c1c7acc99c0eda9accf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723332"
---
# <a name="isymunmanagedvariablegetaddressfield3-method"></a>ISymUnmanagedVariable::GetAddressField3 方法

获取此变量的第三个地址字段。 其含义取决于地址的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAddressField3(  
    [out, retval] ULONG32* pRetVal);  
```  
  
## <a name="parameters"></a>参数  

 `pRetVal`  
 弄指向的指针 `ULONG32` ，该指针接收第三个地址字段。  
  
## <a name="return-value"></a>返回值  

 如果该方法成功，则 S_OK;否则，E_FAIL 或其他一些错误代码。  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [ISymUnmanagedVariable 接口](isymunmanagedvariable-interface.md)
- [GetAddressField1 方法](isymunmanagedvariable-getaddressfield1-method.md)
- [GetAddressField2 方法](isymunmanagedvariable-getaddressfield2-method.md)
- [GetAddressKind 方法](isymunmanagedvariable-getaddresskind-method.md)
