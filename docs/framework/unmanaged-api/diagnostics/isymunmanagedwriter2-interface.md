---
title: ISymUnmanagedWriter2 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter2
helpviewer_keywords:
- ISymUnmanagedWriter2 interface [.NET Framework debugging]
ms.assetid: 8e78faa4-cf43-44fb-a91d-94d6df692a25
topic_type:
- apiref
ms.openlocfilehash: 6feb48b7c78dda64ba372e470b83ffb14f21f2f9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683324"
---
# <a name="isymunmanagedwriter2-interface"></a>ISymUnmanagedWriter2 接口

表示符号编写器，并提供定义文档、序列点、词法范围和变量的方法。 此接口扩展 [ISymUnmanagedWriter](isymunmanagedwriter-interface.md) 接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[DefineConstant2 方法](isymunmanagedwriter2-defineconstant2-method.md)|定义常数值的名称。|  
|[DefineGlobalVariable2 方法](isymunmanagedwriter2-defineglobalvariable2-method.md)|定义单个全局变量。|  
|[DefineLocalVariable2 方法](isymunmanagedwriter2-definelocalvariable2-method.md)|在当前词法范围内定义单个变量。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
- [ISymUnmanagedWriter3 接口](isymunmanagedwriter3-interface.md)
