---
title: CloseEnum 方法
ms.date: 03/30/2017
api_name:
- CloseEnum
- IALink.CloseEnum
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- CloseEnum
helpviewer_keywords:
- CloseEnum method
ms.assetid: aa4a091e-13fe-4264-91de-e12f1c767c87
topic_type:
- apiref
ms.openlocfilehash: 59b1ec3f9ca382ef13680e3aad4d0c0c0e175f1c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716962"
---
# <a name="closeenum-method"></a>CloseEnum 方法

关闭所指示的枚举并释放关联的资源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CloseEnum(  
    HALINKENUM hEnum  
) PURE;  
```  
  
## <a name="parameters"></a>参数  

 `hEnum`  
 要关闭的枚举的句柄。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则返回 S_OK。  
  
## <a name="requirements"></a>要求  

 需要 alink  
  
## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
