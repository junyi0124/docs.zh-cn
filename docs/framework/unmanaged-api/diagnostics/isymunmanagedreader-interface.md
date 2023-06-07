---
title: ISymUnmanagedReader 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader
helpviewer_keywords:
- ISymUnmanagedReader interface [.NET Framework debugging]
ms.assetid: aa3cc15d-058e-4e6f-b03e-39569646ba47
topic_type:
- apiref
ms.openlocfilehash: bca84fdba575ed9bfe572b9fd7a5869620962de6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675862"
---
# <a name="isymunmanagedreader-interface"></a>ISymUnmanagedReader 接口

表示一个符号读取器，该读取器提供对符号存储区中文档、方法和变量的访问。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetDocument 方法](isymunmanagedreader-getdocument-method.md)|查找文档。|  
|[GetDocuments 方法](isymunmanagedreader-getdocuments-method.md)|返回在符号存储区中定义的所有文档的数组。|  
|[GetDocumentVersion 方法](isymunmanagedreader-getdocumentversion-method.md)|获取指定文档的指定版本。|  
|[GetGlobalVariables 方法](isymunmanagedreader-getglobalvariables-method.md)|返回所有全局变量。|  
|[GetMethod 方法](isymunmanagedreader-getmethod-method.md)|在给定方法标记的情况下，获取符号读取器方法。|  
|[GetMethodByVersion 方法](isymunmanagedreader-getmethodbyversion-method.md)|在给定方法标记和编辑和复制版本号的情况下，获取符号读取器方法。|  
|[GetMethodFromDocumentPosition 方法](isymunmanagedreader-getmethodfromdocumentposition-method.md)|返回包含文档中给定位置处的断点的方法。|  
|[GetMethodsFromDocumentPosition 方法](isymunmanagedreader-getmethodsfromdocumentposition-method.md)|返回一组方法，其中每个方法都包含文档中给定位置处的断点。|  
|[GetMethodVersion 方法](isymunmanagedreader-getmethodversion-method.md)|获取方法版本。|  
|[GetNamespaces 方法](isymunmanagedreader-getnamespaces-method.md)|获取在此符号存储区的全局范围内定义的命名空间。|  
|[GetSymAttribute 方法](isymunmanagedreader-getsymattribute-method.md)|根据名称获取自定义属性。|  
|[GetSymbolStoreFileName 方法](isymunmanagedreader-getsymbolstorefilename-method.md)|提供符号存储区的磁盘上的文件名。|  
|[GetUserEntryPoint 方法](isymunmanagedreader-getuserentrypoint-method.md)|返回指定为模块的用户入口点的方法（如果有）。|  
|[GetVariables 方法](isymunmanagedreader-getvariables-method.md)|根据给定的父和名称返回非局部变量。|  
|[Initialize 方法](isymunmanagedreader-initialize-method.md)|用此读取器将与之关联的元数据导入程序接口以及模块的文件名初始化符号读取器。|  
|[ReplaceSymbolStore 方法](isymunmanagedreader-replacesymbolstore-method.md)|用增量符号存储区替换现有的符号存储区。|  
|[UpdateSymbolStore 方法](isymunmanagedreader-updatesymbolstore-method.md)|使用增量符号存储区更新现有的符号存储区。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedReader2 接口](isymunmanagedreader2-interface.md)
