---
title: CreateALink 函数
ms.date: 03/30/2017
api_name:
- CreateALink
api_location:
- alink.dll
api_type:
- DLLExport
f1_keywords:
- CreateALink
helpviewer_keywords:
- CreateALink function
- Alink API, CreateALink function
ms.assetid: fc73bcb9-6af6-44d8-bc39-2f4400325dae
topic_type:
- apiref
ms.openlocfilehash: 98c6ed4657dc69554a9fcca27145f65c621492f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683726"
---
# <a name="createalink-function"></a>CreateALink 函数

创建程序集链接器的实例，并设置指向指定接口的指针。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateALink (  
   REFIID riid,  
   IUnknown **ppInterface  
);  
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|---------------|-----------------|  
|`riid`|某个程序集链接器接口的物理名称。|  
|`ppInterface`|成功完成后的位置包含指向接口的指针 `riid` 。|  
  
## <a name="requirements"></a>要求  

 **库**： alink.dll  
  
## <a name="see-also"></a>另请参阅

- [Al.exe（程序集链接器）](../../tools/al-exe-assembly-linker.md)
