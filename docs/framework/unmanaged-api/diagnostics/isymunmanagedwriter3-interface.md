---
title: ISymUnmanagedWriter3 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter3
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter3
helpviewer_keywords:
- ISymUnmanagedWriter3 interface [.NET Framework debugging]
ms.assetid: a034c21e-e371-4360-b470-29e88288948f
topic_type:
- apiref
ms.openlocfilehash: dfc2e39a6a39e7386bd7358d422d5c6978ec42ec
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683285"
---
# <a name="isymunmanagedwriter3-interface"></a>ISymUnmanagedWriter3 接口

表示符号编写器，并提供定义文档、序列点、词法范围和变量的方法。 此接口扩展 [ISymUnmanagedWriter](isymunmanagedwriter-interface.md) 接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Commit 方法](isymunmanagedwriter3-commit-method.md)|将迄今为止写入的更改提交到流。|  
|[OpenMethod2 方法](isymunmanagedwriter3-openmethod2-method.md)|打开方法并在图像中提供其实际节偏移量。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)
- [ISymUnmanagedWriter2 接口](isymunmanagedwriter2-interface.md)
