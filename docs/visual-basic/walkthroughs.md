---
title: 语言演练
description: Visual Basic 开发中常见方案的逐步说明
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic, walkthroughs
- examples [Visual Basic]
- Visual Basic code, walkthroughs
- walkthroughs [Visual Basic]
ms.assetid: e4e1f849-e1ce-4cf7-8483-d9b4c4887a8e
ms.openlocfilehash: 76f9b428bc5f613296e24d893f49f124bb13c089
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "75636037"
---
# <a name="visual-basic-language-walkthroughs"></a>Visual Basic 语言演练

演练提供有关常见方案的分步说明，这使它们成为了解产品或特定功能部分的良好开端。

- [编写异步程序](./programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)  
 演示如何使用 [Async](language-reference/modifiers/async.md) 和 [Await](language-reference/operators/await-operator.md) 创建异步解决方案。

- [声明和引发事件](programming-guide/language-features/events/walkthrough-declaring-and-raising-events.md)  
 演示如何在 Visual Basic 中声明和引发事件。

- [处理事件](programming-guide/language-features/events/walkthrough-handling-events.md)  
 演示如何使用标准的 `WithEvents` 关键字或新的 `AddHandler`/`RemoveHandler` 关键字处理事件。

- [创建和实现接口](programming-guide/language-features/interfaces/walkthrough-creating-and-implementing-interfaces.md)  
 演示如何在 Visual Basic 中声明和实现接口。

- [定义类](programming-guide/language-features/objects-and-classes/walkthrough-defining-classes.md)  
 描述如何声明类及其字段、属性、方法和事件。

- [用 Visual Basic 编写查询](programming-guide/concepts/linq/walkthrough-writing-queries.md)  
 演示如何使用 Visual Basic 语言功能编写语言集成查询 (LINQ) 查询表达式。

- [在 Visual Basic 中实现 IEnumerable(Of T)](programming-guide/language-features/control-flow/walkthrough-implementing-ienumerable-of-t.md)  
 演示如何创建实现 `IEnumerable(Of String)` 接口的类和实现 `IEnumerator(Of String)` 接口的类，以一次读取一行文本文件。

- [调用 Windows API](programming-guide/com-interop/walkthrough-calling-windows-apis.md)  
 说明如何使用 `Declare` 语句并调用 Windows API。 包括以下信息：使用特性来控制 API 调用的封送，以及如何将 API 调用公开为类的方法。

- [使用 Visual Basic 创建 COM 对象](programming-guide/com-interop/walkthrough-creating-com-objects.md)  
 演示如何在 Visual Basic 中（使用或不使用 COM 类模板）创建 COM 对象。

- [使用 COM 对象实现继承](programming-guide/com-interop/walkthrough-implementing-inheritance-with-com-objects.md)  
 演示如何使用 Visual Basic 6.0 创建包含类的 COM 对象，然后将其用作 Visual Basic 中的基类。

- [确定 My.Application.Log 写入信息的位置](developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)  
 介绍默认 `My.Application.Log` 设置以及如何确定应用程序的设置。

- [更改 My.Application.Log 写入信息的位置](developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)  
 演示如何重写日志记录事件信息的默认 `My.Application.Log` 和 `My.Log` 设置，以及如何使 `Log` 对象将信息写入其他日志侦听器。

- [筛选 My.Application.Log 输出](developing-apps/programming/log-info/walkthrough-filtering-my-application-log-output.md)  
 演示如何更改 `My.Application.Log` 对象的默认日志筛选。

- [创建自定义日志侦听器](developing-apps/programming/log-info/walkthrough-creating-custom-log-listeners.md)  
 演示如何创建自定义日志侦听器，并将其配置为侦听 `My.Application.Log` 对象的输出。

- [嵌入托管程序集中的类型](../standard/assembly/embed-types-visual-studio.md)  
 说明如何创建程序集和从其中嵌入类型的客户端程序。

- [验证密码是否复杂 (Visual Basic)](programming-guide/language-features/strings/walkthrough-validating-that-passwords-are-complex.md)  
 演示如何检查强密码特性，及如何更新提供有关检查密码失败的特性的信息的字符串参数。

- [在 Visual Basic 中对字符串进行加密和解密](programming-guide/language-features/strings/walkthrough-encrypting-and-decrypting-strings.md)  
 演示如何使用 <xref:System.Security.Cryptography.DESCryptoServiceProvider> 类加密和解密字符串。

- [在 Visual Basic 中操作文件和文件夹](developing-apps/programming/drives-directories-files/walkthrough-manipulating-files-and-directories.md)  
 演示如何使用 Visual Basic 函数确定文件信息、在文件中搜索字符串以及对文件进行写入操作。

- [使用 .NET Framework 方法操作文件](developing-apps/programming/drives-directories-files/walkthrough-manipulating-files-by-using-net-framework-methods.md)  
 演示如何使用 .NET Framework 方法确定文件信息，在文件中搜索字符串，以及对文件进行写入操作。

- [在 Visual Basic 中保持对象](programming-guide/concepts/serialization/walkthrough-persisting-an-object-in-visual-studio.md)  
 演示如何创建简单的对象并将其数据保存到文件中。

- [演练：带有“使用时生成”功能的测试先行支持](/visualstudio/ide/walkthrough-test-first-support-with-the-generate-from-usage-feature)  
 演示执行测试先行开发的方式，即首先编写单元测试，然后编写使测试成功的源代码。
