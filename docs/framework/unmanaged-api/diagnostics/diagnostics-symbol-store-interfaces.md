---
title: 诊断符号存储区接口
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], debugging
- diagnostics symbol store interfaces [.NET Framework]
- interfaces [.NET Framework], diagnostics symbol store
- unmanaged interfaces [.NET Framework], diagnostics symbol store
- debugging interfaces [.NET Framework]
- interfaces [.NET Framework debugging]
ms.assetid: f96987d5-e6a5-478b-ac5e-302e16545cce
ms.openlocfilehash: e376544a9d428ce5110a7e38b92a8e830f574664
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725178"
---
# <a name="diagnostics-symbol-store-interfaces"></a>诊断符号存储区接口

本主题介绍了一些非托管接口，这些接口允许编译器生成符号信息以供调试器使用。  
  
## <a name="in-this-section"></a>本节内容  

 [IBindingDisplay 接口](ibindingdisplay-interface.md)  
 提供显示有关正在运行的应用程序的当前绑定信息的方法。  
  
 [IDebugAutoAttach 接口](idebugautoattach-interface.md)  
 定义服务器调用的调试器自动附加的接口。  
  
 [INotifyConnection2 接口](inotifyconnection2-interface.md)  
 声明用于注册和注销连接通知源的方法。  
  
 [INotifySink2 接口](inotifysink2-interface.md)  
 声明接收器通知的方法。  
  
 [INotifySource2 接口](inotifysource2-interface.md)  
 声明用于设置通知筛选器的方法。  
  
 [ISymENCUnmanagedMethod 接口](isymencunmanagedmethod-interface.md)  
 提供 "编辑并继续" 功能的信息。  
  
 [ISymUnmanagedAsyncMethod 接口](isymunmanagedasyncmethod-interface.md)  
 此接口是 [ISymUnmanagedAsyncMethodPropertiesWriter 接口](isymunmanagedasyncmethodpropertieswriter-interface.md)的读取补充。  
  
 [ISymUnmanagedAsyncMethodPropertiesWriter 接口](isymunmanagedasyncmethodpropertieswriter-interface.md)  
 允许为每个方法符号定义可选的异步方法信息。 必须将与已打开的方法一起使用 (即，在对 [OpenMethod 方法](isymunmanagedwriter-openmethod-method.md)的调用与 [CloseMethod 方法](isymunmanagedwriter-closemethod-method.md) 的调用之间) 。  
  
 [ISymUnmanagedBinder 接口](isymunmanagedbinder-interface.md)  
 表示非托管代码的符号绑定器。  
  
 [ISymUnmanagedBinder2 接口](isymunmanagedbinder2-interface.md)  
 表示非托管代码的符号联编程序，并扩展 `ISymUnmanagedBinder` 接口。  
  
 [ISymUnmanagedBinder3 接口](isymunmanagedbinder3-interface.md)  
 表示非托管代码的符号联编程序，并扩展 `ISymUnmanagedBinder` 接口。  
  
 [ISymUnmanagedConstant 接口](isymunmanagedconstant-interface.md)  
 提供对非托管常数的访问。  
  
 [ISymUnmanagedDispose 接口](isymunmanageddispose-interface.md)  
 释放非托管资源。  
  
 [ISymUnmanagedDocument 接口](isymunmanageddocument-interface.md)  
 表示由符号存储引用的文档。  
  
 [ISymUnmanagedDocumentWriter 接口](isymunmanageddocumentwriter-interface.md)  
 提供用于写入到符号存储区引用的文档的方法。  
  
 [ISymUnmanagedENCUpdate 接口](isymunmanagedencupdate-interface.md)  
 提供 "编辑并继续" 功能的方法。  
  
 [ISymUnmanagedMethod 接口](isymunmanagedmethod-interface.md)  
 表示符号存储区中的方法。  
  
 [ISymUnmanagedNamespace 接口](isymunmanagednamespace-interface.md)  
 表示命名空间。  
  
 [ISymUnmanagedReader 接口](isymunmanagedreader-interface.md)  
 表示一个符号读取器，该读取器提供对符号存储区中文档、方法和变量的访问。  
  
 [ISymUnmanagedReader2 接口](isymunmanagedreader2-interface.md)  
 给定方法标记和编辑和复制版本号，获取符号读取器方法。  
  
 [ISymUnmanagedReaderSymbolSearchInfo 接口](isymunmanagedreadersymbolsearchinfo-interface.md)  
 提供用于获取符号搜索信息的方法。  
  
 [ISymUnmanagedScope 接口](isymunmanagedscope-interface.md)  
 表示方法中的词法范围。  
  
 [ISymUnmanagedScope2 接口](isymunmanagedscope2-interface.md)  
 表示方法内的词法范围，并 `ISymUnmanagedScope` 使用获取有关范围中定义的常量的信息的方法扩展接口。  
  
 [ISymUnmanagedSourceServerModule 接口](isymunmanagedsourceservermodule-interface.md)  
 提供模块的源服务器数据。  
  
 [ISymUnmanagedSymbolSearchInfo 接口](isymunmanagedsymbolsearchinfo-interface.md)  
 提供获取有关搜索路径的信息的方法。  
  
 [ISymUnmanagedVariable 接口](isymunmanagedvariable-interface.md)  
 表示变量，如参数、局部变量或字段。  
  
 [ISymUnmanagedWriter 接口](isymunmanagedwriter-interface.md)  
 表示符号编写器，并提供定义文档、序列点、词法范围和变量的方法。  
  
 [ISymUnmanagedWriter2 接口](isymunmanagedwriter2-interface.md)  
 表示符号编写器，并提供定义文档、序列点、词法范围和变量的方法。 扩展 `ISymUnmanagedWriter` 接口。  
  
 [ISymUnmanagedWriter3 接口](isymunmanagedwriter3-interface.md)  
 表示符号编写器，并提供定义文档、序列点、词法范围和变量的方法。 扩展 `ISymUnmanagedWriter` 接口。  
  
 [ISymUnmanagedWriter4 接口](isymunmanagedwriter4-interface.md)  
 ISymUnmanagedWriter4 接口。  
  
 [ISymUnmanagedWriter5 接口](isymunmanagedwriter5-interface.md)  
 ISymUnmanagedWriter5 接口。  
  
## <a name="related-sections"></a>相关章节  

 [诊断符号存储区枚举](diagnostics-symbol-store-enumerations.md)  
  
 [诊断符号存储区结构](diagnostics-symbol-store-structures.md)  
  
 [调试](../debugging/index.md)
