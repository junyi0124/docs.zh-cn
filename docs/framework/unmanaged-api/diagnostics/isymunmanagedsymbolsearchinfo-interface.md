---
title: ISymUnmanagedSymbolSearchInfo 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedSymbolSearchInfo
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedSymbolSearchInfo
helpviewer_keywords:
- ISymUnmanagedSymbolSearchInfo interface [.NET Framework debugging]
ms.assetid: 30817373-0a21-49c1-a0c4-8e8daeecb8db
topic_type:
- apiref
ms.openlocfilehash: 95ad3cbea4269173f22e662d15772ff97f7ee900
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705444"
---
# <a name="isymunmanagedsymbolsearchinfo-interface"></a>ISymUnmanagedSymbolSearchInfo 接口

提供获取有关搜索路径的信息的方法。 通过 `QueryInterface` 对实现 [ISymUnmanagedReader](isymunmanagedreader-interface.md) 接口的对象调用来获取此接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetHRESULT 方法](isymunmanagedsymbolsearchinfo-gethresult-method.md)|获取 HRESULT。|  
|[GetSearchPath 方法](isymunmanagedsymbolsearchinfo-getsearchpath-method.md)|获取搜索路径。|  
|[GetSearchPathLength 方法](isymunmanagedsymbolsearchinfo-getsearchpathlength-method.md)|获取搜索路径长度。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
