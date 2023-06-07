---
title: IMetaDataError 接口
ms.date: 03/30/2017
api_name:
- IMetaDataError
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataError
helpviewer_keywords:
- IMetaDataError interface [.NET Framework metadata]
ms.assetid: 0020b62c-ea88-40c7-a9ee-16b064f81624
topic_type:
- apiref
ms.openlocfilehash: 5f5e04787ce0ab0e1c8ecf3c19ba37e76ba38bfe
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701921"
---
# <a name="imetadataerror-interface"></a>IMetaDataError 接口

提供用于在元数据合并期间报告错误的回调机制。  
  
> [!NOTE]
> `IMetaDataError`接口必须由客户端实现。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[OnError 方法](imetadataerror-onerror-method.md)|提供在元数据合并期间发生的错误的通知。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
