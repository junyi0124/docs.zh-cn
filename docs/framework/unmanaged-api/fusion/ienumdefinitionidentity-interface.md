---
title: IEnumDefinitionIdentity 接口
ms.date: 03/30/2017
api_name:
- IEnumDefinitionIdentity
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IEnumDefinitionIdentity
helpviewer_keywords:
- IEnumDefinitionIdentity interface [.NET Framework fusion]
ms.assetid: 8263e75d-251b-4abc-8a1a-c62884142232
topic_type:
- apiref
ms.openlocfilehash: f3872a2b03d3b22d695af1c104e9ae8ba8856990
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729000"
---
# <a name="ienumdefinitionidentity-interface"></a>IEnumDefinitionIdentity 接口

用作对象集合的枚举器 `IDefinitionIdentity` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
IEnumDefinitionIdentity : IUnknown {  
  
    HRESULT Clone (  
        [out] IEnumDefinitionIdentity **ppIEnumDefinitionIdentity  
    );  
  
    HRESULT Next (  
        [in]  ULONG               celt,  
        [out, length_is(celt), size_is(*pceltWritten)]  
              IDefinitionIdentity *rgpIDefinitionIdentity[],  
        [out] ULONG               *pceltWritten  
    );  
  
    HRESULT Reset ();  
  
    HRESULT Skip (  
        [in] ULONG celt  
    );  
  
};  
```  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|`IEnumDefinitionIdentity::Clone`|获取指向新对象的接口指针 `IEnumDefinitionIdentity` ，该对象包含与此相同的成员 `IEnumDefinitionIdentity` 。|  
|`IEnumDefinitionIdentity::Next`|`IDefinitionIdentity`从当前位置开始，获取指定数目的对象。|  
|`IEnumDefinitionIdentity::Reset`|将指令指针移到此的开头 `IEnumDefinitionIdentity` 。|  
|`IEnumDefinitionIdentity::Skip`|从当前位置开始，将指令指针向前移动指定数量的元素。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 隔离。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
- [IDefinitionIdentity 接口](idefinitionidentity-interface.md)
