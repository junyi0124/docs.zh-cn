---
title: ISymUnmanagedBinder2 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedBinder2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedBinder2 Interface
helpviewer_keywords:
- ISymUnmanagedBinder2 interface [.NET Framework debugging]
ms.assetid: 7a59f405-73e8-4434-8bcc-a9dc45ea08e6
topic_type:
- apiref
ms.openlocfilehash: c5a43f6c277f582f9f14cefe5bfba6f5300c09d8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727349"
---
# <a name="isymunmanagedbinder2-interface"></a>ISymUnmanagedBinder2 接口

表示非托管代码的符号联编程序，并扩展 [ISymUnmanagedBinder](isymunmanagedbinder-interface.md) 接口。  
  
> [!IMPORTANT]
> 从不受信任的源中打开程序数据库 (PDB) 文件会带来安全风险。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetReaderForFile2 方法](isymunmanagedbinder2-getreaderforfile2-method.md)|给定元数据接口和文件名后，将返回正确的 [ISymUnmanagedReader](isymunmanagedreader-interface.md) 接口，该接口将读取与模块关联的调试符号。 与 [ISymUnmanagedBinder：： GetReaderForFile](isymunmanagedbinder-getreaderforfile-method.md) 方法相比，提供的搜索范围更广。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedBinder 接口](isymunmanagedbinder-interface.md)
- [ISymUnmanagedBinder3 接口](isymunmanagedbinder3-interface.md)
