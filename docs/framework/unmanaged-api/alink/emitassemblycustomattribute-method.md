---
title: EmitAssemblyCustomAttribute 方法
ms.date: 03/30/2017
api_name:
- IALink.EmitAssemblyCustomAttribute
- EmitAssemblyCustomAttribute
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EmitAssemblyCustomAttribute
helpviewer_keywords:
- EmitAssemblyCustomAttribute method
ms.assetid: b72f5409-79af-4fa7-90a7-7630eec170f1
topic_type:
- apiref
ms.openlocfilehash: 2070d1ec2aec80638c20c764eed5086c4a42e0fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676356"
---
# <a name="emitassemblycustomattribute-method"></a>EmitAssemblyCustomAttribute 方法

调用以设置程序集级别的自定义特性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EmitAssemblyCustomAttribute(  
    mdAssembly   AssemblyID,  
    mdToken      FileToken,  
    mdToken      tkType,  
    void const*  pCustomValue,  
    DWORD        cbCustomValue,  
    BOOL         bSecurity,  
    BOOL         bAllowMulti  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `AssemblyID`  
 程序集的 ID。  
  
 `FileToken`  
 定义属性的文件。 如果不 `AssemblyID` 指示未绑定的 .netmodule，则可以为 NULL。  
  
 `tkType`  
 自定义属性的类型。  
  
 `pCustomValue`  
 自定义值数据。  
  
 `cbCustomValue`  
 自定义值数据的长度。  
  
 `bSecurity`  
 如果自定义属性与程序集签名相关，则为 TRUE。  
  
 `bAllowMulti`  
 如果要发出多个属性，则为 TRUE。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
