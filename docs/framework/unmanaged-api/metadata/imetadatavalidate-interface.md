---
title: IMetaDataValidate 接口
ms.date: 03/30/2017
api_name:
- IMetaDataValidate
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataValidate
helpviewer_keywords:
- IMetaDataValidate interface [.NET Framework metadata]
ms.assetid: db98608a-e85c-4f50-9d7b-5f57a426ddb6
topic_type:
- apiref
ms.openlocfilehash: 518ee65bc684f643bf4f608223c0fa40ea3f0dd9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685521"
---
# <a name="imetadatavalidate-interface"></a>IMetaDataValidate 接口

提供验证元数据签名的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ValidateMetaData 方法](imetadatavalidate-validatemetadata-method.md)|验证在当前元数据范围内的对象的元数据签名。|  
|[ValidatorInit 方法](imetadatavalidate-validatorinit-method.md)|设置一个标志，该标志指定当前元数据范围内的模块类型，并注册验证错误的指定回调方法。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
