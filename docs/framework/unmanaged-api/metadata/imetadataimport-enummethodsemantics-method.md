---
title: IMetaDataImport::EnumMethodSemantics 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMethodSemantics
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMethodSemantics
helpviewer_keywords:
- EnumMethodSemantics method [.NET Framework metadata]
- IMetaDataImport::EnumMethodSemantics method [.NET Framework metadata]
ms.assetid: e7e3c630-9691-46d6-94df-b5593a7bb08a
topic_type:
- apiref
ms.openlocfilehash: 3d14aea92633c944d21d867c8152767ae6f1f291
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720966"
---
# <a name="imetadataimportenummethodsemantics-method"></a>IMetaDataImport::EnumMethodSemantics 方法

枚举与指定方法相关的属性和属性更改事件。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumMethodSemantics (  
   [in, out] HCORENUM    *phEnum,  
   [in]  mdMethodDef     mb,
   [out] mdToken         rEventProp[],  
   [in]  ULONG           cMax,  
   [out] ULONG           *pcEventProp  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。 第一次调用此方法时，此值必须为 NULL。  
  
 `mb`  
 中用于限制枚举范围的 MethodDef 标记。  
  
 `rEventProp`  
 弄用于存储事件或属性的数组。  
  
 `cMax`  
 [in] `rEventProp` 数组的最大大小。  
  
 `pcEventProp`  
 弄中返回的事件或属性的数目 `rEventProp` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumMethodSemantics` 已成功返回。|  
|`S_FALSE`|没有要枚举的事件或属性。 在这种情况下， `pcEventProp` 为零。|  
  
## <a name="remarks"></a>注解  

 很多公共语言运行时 *Property* 类型定义 `Changed` 与其属性相关的属性事件和 `On` *属性* `Changed` 方法。 例如， <xref:System.Windows.Forms.Control?displayProperty=nameWithType> 类型定义 <xref:System.Windows.Forms.Control.Font%2A> 属性、 <xref:System.Windows.Forms.Control.FontChanged> 事件和 <xref:System.Windows.Forms.Control.OnFontChanged%2A> 方法。 属性的 set 访问器方法 <xref:System.Windows.Forms.Control.Font%2A> 调用 <xref:System.Windows.Forms.Control.OnFontChanged%2A> 方法，而后者又引发了 <xref:System.Windows.Forms.Control.FontChanged> 事件。 您将 `EnumMethodSemantics` 使用的 MethodDef 调用 <xref:System.Windows.Forms.Control.OnFontChanged%2A> 来获取对 <xref:System.Windows.Forms.Control.Font%2A> 属性和事件的引用 <xref:System.Windows.Forms.Control.FontChanged> 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
