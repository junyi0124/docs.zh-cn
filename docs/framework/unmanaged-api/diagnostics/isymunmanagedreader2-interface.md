---
title: ISymUnmanagedReader2 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader2
helpviewer_keywords:
- ISymUnmanagedReader2 interface [.NET Framework debugging]
ms.assetid: a01a881b-82a3-4b3e-a3a9-9dc305c2e5f7
topic_type:
- apiref
ms.openlocfilehash: 3f34be833d3ccb5c636d2c5f18ccb6e216ef2c49
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709071"
---
# <a name="isymunmanagedreader2-interface"></a>ISymUnmanagedReader2 接口

表示一个符号读取器，该读取器提供对符号存储区中文档、方法和变量的访问。 此接口扩展 [ISymUnmanagedReader](isymunmanagedreader-interface.md) 接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetMethodByVersionPreRemap 方法](isymunmanagedreader2-getmethodbyversionpreremap-method.md)|在给定方法标记和编辑并继续版本号的情况下，获取符号读取器方法。|  
|[GetMethodsInDocument 方法](isymunmanagedreader2-getmethodsindocument-method.md)|获取所提供文档中包含行信息的每个方法。|  
|[GetSymAttributePreRemap 方法](isymunmanagedreader2-getsymattributepreremap-method.md)|根据名称获取自定义属性。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedReader 接口](isymunmanagedreader-interface.md)
