---
title: IMetaDataEmit2 接口
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2
helpviewer_keywords:
- IMetaDataEmit2 interface [.NET Framework metadata]
ms.assetid: 866dc96b-bbfc-4c0f-80c2-38ce93072106
topic_type:
- apiref
ms.openlocfilehash: b8ad159dace734c343297b256092162f17ab829b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726478"
---
# <a name="imetadataemit2-interface"></a>IMetaDataEmit2 接口

扩展 [IMetaDataEmit](imetadataemit-interface.md) 接口，主要用于提供使用泛型类型的能力。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[DefineGenericParam 方法](imetadataemit2-definegenericparam-method.md)|创建泛型类型参数的定义，并获取该泛型类型参数的令牌。|  
|[DefineMethodSpec 方法](imetadataemit2-definemethodspec-method.md)|创建方法的泛型实例，并获取定义的标记。|  
|[GetDeltaSaveSize 方法](imetadataemit2-getdeltasavesize-method.md)|获取一个值，该值指示为当前的 "编辑并继续" 会话表示更改所需的数据的大小差异。|  
|[ResetENCLog 方法](imetadataemit2-resetenclog-method.md)|重置编辑并继续日志并启动新的会话。|  
|[SaveDelta 方法](imetadataemit2-savedelta-method.md)|将当前的 "编辑并继续" 会话中的更改保存到指定的文件。|  
|[SaveDeltaToMemory 方法](imetadataemit2-savedeltatomemory-method.md)|将当前的 "编辑并继续" 会话中的更改保存到内存。|  
|[SaveDeltaToStream 方法](imetadataemit2-savedeltatostream-method.md)|将当前的 "编辑并继续" 会话中的更改保存到指定的流。|  
|[SetGenericParamProps 方法](imetadataemit2-setgenericparamprops-method.md)|设置指定的标记所引用的泛型参数定义的属性值。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
- [IMetaDataEmit 接口](imetadataemit-interface.md)
