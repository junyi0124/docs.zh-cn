---
title: 异常类和属性
ms.date: 03/30/2017
helpviewer_keywords:
- exceptions, Exception class
- Exception class
ms.assetid: e2e1f8c4-e7b4-467d-9a66-13c90861221d
ms.openlocfilehash: 459dc15f136c98d4b889a420dbd863b16aca747c
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828200"
---
# <a name="exception-class-and-properties"></a>异常类和属性

<xref:System.Exception> 类是异常从中继承的基类。 例如，<xref:System.InvalidCastException> 类层次结构如下所示：

<xref:System.Object>\
&nbsp;&nbsp;<xref:System.Exception>\
&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.SystemException>\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<xref:System.InvalidCastException>

<xref:System.Exception> 类具有以下属性，有助于更轻松地理解异常。

| 属性名称 | 说明 |
| ------------- | ----------- |
| <xref:System.Exception.Data> | <xref:System.Collections.IDictionary> 包含键/值对中的任意数据。 |
| <xref:System.Exception.HelpLink> | 可容纳指向帮助文件的 URL（或 URN），帮助文件中提供了大量信息说明了异常的原因。 |
| <xref:System.Exception.InnerException> | 在处理异常时此属性可用于创建和保留一系列异常。 可将其用于创建新异常，其中包含之前捕获到的异常。 可通过 <xref:System.Exception.InnerException> 属性中的第二个异常捕获原始异常，允许处理第二个异常的代码检查其他信息。 例如，假设有一个方法可接收格式不正确的参数。  该代码尝试读取参数，但会引发异常。 该方法会捕获异常并引发 <xref:System.FormatException>。 若要提高调用方确定引发异常的原因的能力，有时最好用一个方法捕获帮助器例程引发的异常，然后引发一个更能说明已发生错误的异常。 可创建一个新的且更有意义的异常，其中可将内部异常引用设为原始异常。 然后可向调用方引发此更有意义的异常。 请注意，使用此功能，可创建一系列链接的异常，以最先引发的异常结尾。 |
| <xref:System.Exception.Message> | 提供有关异常原因的详细信息。
| <xref:System.Exception.Source> | 获取或设置导致错误的应用程序或对象的名称。 |
| <xref:System.Exception.StackTrace>| 包含可用于确定错误位置的堆栈跟踪。 如果有可用的调试信息，则堆栈跟踪包含源文件名和程序行号。 |

继承自 <xref:System.Exception> 的大多数类无法实现其他成员或提供其他功能；它们只从 <xref:System.Exception> 进行继承。 因此，可在异常类层次结构、异常名称和异常所含的信息中找到异常的重要信息。

建议仅抛出和捕获派生自 <xref:System.Exception> 的对象，但可以将派生自 <xref:System.Object> 类的任何对象作为异常抛出。 请注意，并非所有语言都支持引发和捕获不是从 <xref:System.Exception> 派生的对象。
  
## <a name="see-also"></a>另请参阅

- [异常](index.md)
