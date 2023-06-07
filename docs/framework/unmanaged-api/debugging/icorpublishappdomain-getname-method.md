---
title: ICorPublishAppDomain::GetName 方法
ms.date: 03/30/2017
api_name:
- ICorPublishAppDomain.GetName
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublishAppDomain::GetName
helpviewer_keywords:
- GetName method, ICorPublishAppDomain interface [.NET Framework debugging]
- ICorPublishAppDomain::GetName method [.NET Framework debugging]
ms.assetid: 6ef8ac9b-9803-4b65-8b13-25f3e0b1bc6b
topic_type:
- apiref
ms.openlocfilehash: d6b05333b9e02c4202c0fd9bdee9b5c055aa4da3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95694355"
---
# <a name="icorpublishappdomaingetname-method"></a>ICorPublishAppDomain::GetName 方法

获取此 [ICorPublishAppDomain](icorpublishappdomain-interface.md)所表示的应用程序域的名称。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetName (  
    [in]  ULONG32   cchName,
    [out] ULONG32   *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]
        WCHAR       *szName  
);  
```  
  
## <a name="parameters"></a>参数  

 `cchName`  
 [in] `szName` 数组的大小。  
  
 `pcchName`  
 弄一个指针，它指向数组中返回的宽字符数，包括 null 字符 `szName` 。  
  
 `szName`  
 弄要在其中存储名称的数组。  
  
## <a name="remarks"></a>注解  

 如果 `szName` 为非 null，则该 `GetName` 方法将最多复制 `cchName` (个字符，包括 null 结束符) 入 `szName` 。 如果在中返回非 null，则 `pcchName` 名称中的实际字符数 (包括 null 结束符) 存储在 `szName` 数组中。  
  
 `GetName`无论复制了多少个字符，该方法都将返回 S_OK HRESULT。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CorPub，CorPub  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorPublishAppDomain 接口](icorpublishappdomain-interface.md)
