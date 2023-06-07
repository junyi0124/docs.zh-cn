---
title: ISymUnmanagedWriter 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter
helpviewer_keywords:
- ISymUnmanagedWriter interface [.NET Framework debugging]
ms.assetid: 7d6733ec-f081-4166-bc17-de09e16dc304
topic_type:
- apiref
ms.openlocfilehash: fddfd2a163f6e6513b648ee0b724c0b5bd54c81a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722929"
---
# <a name="isymunmanagedwriter-interface"></a>ISymUnmanagedWriter 接口

表示符号编写器，并提供定义文档、序列点、词法范围和变量的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Abort 方法](isymunmanagedwriter-abort-method.md)|关闭符号编写器，而不将符号提交到符号存储区。|  
|[Close 方法](isymunmanagedwriter-close-method.md)|将符号提交到符号存储区后关闭符号编写器。|  
|[CloseMethod 方法](isymunmanagedwriter-closemethod-method.md)|关闭当前方法。 方法关闭后，不能在其中定义更多符号。|  
|[CloseNamespace 方法](isymunmanagedwriter-closenamespace-method.md)|关闭最近打开的命名空间。|  
|[CloseScope 方法](isymunmanagedwriter-closescope-method.md)|关闭当前词法范围。|  
|[DefineConstant 方法](isymunmanagedwriter-defineconstant-method.md)|定义常数值的名称。|  
|[DefineDocument 方法](isymunmanagedwriter-definedocument-method.md)|定义源文档。|  
|[DefineField 方法](isymunmanagedwriter-definefield-method.md)|定义不在方法中的单个变量。|  
|[DefineGlobalVariable 方法](isymunmanagedwriter-defineglobalvariable-method.md)|定义单个全局变量。|  
|[DefineLocalVariable 方法](isymunmanagedwriter-definelocalvariable-method.md)|在当前词法范围内定义单个变量。|  
|[DefineParameter 方法](isymunmanagedwriter-defineparameter-method.md)|在当前方法中定义单个参数。|  
|[DefineSequencePoints 方法](isymunmanagedwriter-definesequencepoints-method.md)|在当前方法内定义一组序列点。|  
|[GetDebugInfo 方法](isymunmanagedwriter-getdebuginfo-method.md)|返回编译器将 (PE 中的调试目录项写入) 文件头所需的信息。|  
|[Initialize 方法](isymunmanagedwriter-initialize-method.md)|设置此编写器将与之关联的元数据发射器接口，并设置调试符号将写入的输出文件名。|  
|[Initialize2 方法](isymunmanagedwriter-initialize2-method.md)|设置此编写器将与之关联的元数据发射器接口，设置调试符号将写入到的输出文件名，并将程序数据库 (PDB) 文件的最终位置设置为。|  
|[OpenMethod 方法](isymunmanagedwriter-openmethod-method.md)|打开要在其中发出符号信息的方法。|  
|[OpenNamespace 方法](isymunmanagedwriter-opennamespace-method.md)|打开一个新的命名空间。|  
|[OpenScope 方法](isymunmanagedwriter-openscope-method.md)|在当前方法中打开新的词法范围。|  
|[RemapToken 方法](isymunmanagedwriter-remaptoken-method.md)|通知符号编写器在发出元数据时已重新映射元数据标记。|  
|[SetMethodSourceRange 方法](isymunmanagedwriter-setmethodsourcerange-method.md)|指定源文件内方法的真正开始和结尾。|  
|[SetScopeRange 方法](isymunmanagedwriter-setscoperange-method.md)|定义指定词法范围的偏移量范围。|  
|[SetSymAttribute 方法](isymunmanagedwriter-setsymattribute-method.md)|基于名称定义自定义属性。|  
|[SetUserEntryPoint 方法](isymunmanagedwriter-setuserentrypoint-method.md)|指定用户定义的方法，该方法是此模块的入口点。|  
|[UsingNamespace 方法](isymunmanagedwriter-usingnamespace-method.md)|指定在当前打开的词法范围内使用给定的完全限定的命名空间名称。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedWriter2 接口](isymunmanagedwriter2-interface.md)
- [ISymUnmanagedWriter3 接口](isymunmanagedwriter3-interface.md)
