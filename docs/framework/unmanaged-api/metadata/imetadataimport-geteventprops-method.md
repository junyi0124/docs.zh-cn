---
title: IMetaDataImport::GetEventProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetEventProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetEventProps
helpviewer_keywords:
- IMetaDataImport::GetEventProps method [.NET Framework metadata]
- GetEventProps method [.NET Framework metadata]
ms.assetid: 5eaf3b4a-92b7-4d5b-97e0-1e83721e0052
topic_type:
- apiref
ms.openlocfilehash: 369335deb67d74ee3cb9fa407533e40716aa3a3a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676852"
---
# <a name="imetadataimportgeteventprops-method"></a>IMetaDataImport::GetEventProps 方法

获取指定事件标记所表示的事件的元数据信息，包括声明类型、委托的添加和移除方法以及任何标志和其他关联的数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetEventProps (  
   [in]  mdEvent       ev,  
   [out] mdTypeDef     *pClass,
   [out] LPCWSTR       szEvent,
   [in]  ULONG         cchEvent,
   [out] ULONG         *pchEvent,
   [out] DWORD         *pdwEventFlags,  
   [out] mdToken       *ptkEventType,  
   [out] mdMethodDef   *pmdAddOn,
   [out] mdMethodDef   *pmdRemoveOn,
   [out] mdMethodDef   *pmdFire,
   [out] mdMethodDef   rmdOtherMethod[],
   [in]  ULONG         cMax,  
   [out] ULONG         *pcOtherMethod  
);  
```  
  
## <a name="parameters"></a>参数  

 `ev`  
 中表示要获取其元数据的事件的事件元数据标记。  
  
 `pClass`  
 弄一个指针，指向表示声明事件的类的 TypeDef 标记。  
  
 `szEvent`  
 弄引用的事件的名称 `ev` 。  
  
 `pchEvent`  
 中请求的长度（以宽字符为范围） `szEvent` 。  
  
 `pdwEventFlags`  
 弄返回的宽字符的长度 `szEvent` 。  
  
 `ptkEventType`  
 弄指向表示事件类型的 TypeRef 或 TypeDef 元数据标记的指针 <xref:System.Delegate> 。  
  
 `pmdAddOn`  
 弄指向表示添加事件处理程序的方法的元数据标记的指针。  
  
 `pmdRemoveOn`  
 弄指向表示删除事件处理程序的方法的元数据标记的指针。  
  
 `pmdFire`  
 弄指向表示引发事件的方法的元数据标记的指针。  
  
 `rmdOtherMethod`  
 弄指向与事件关联的其他方法的标记指针的数组。  
  
 `cMax`  
 [in] `rmdOtherMethod` 数组的最大大小。  
  
 `pcOtherMethod`  
 弄返回的令牌数 `rmdOtherMethod` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
