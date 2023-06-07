---
title: 动态语言运行时概述 | Microsoft Docs
description: 阅读 .NET 中的动态语言运行时 (DLR) 概述。 DLR 是一种运行时环境，可以将一组动态语言服务添加到 CLR。
ms.date: 03/30/2017
helpviewer_keywords:
- dynamic language runtime
- IronPython
- DLR
- IronRuby
ms.assetid: f769a271-8aff-4bea-bfab-6160217ce23d
ms.openlocfilehash: a36d6704b36b5006f19e21932797f21af849e55a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266826"
---
# <a name="dynamic-language-runtime-overview"></a>动态语言运行时概述

动态语言运行时 (DLR) 是一种运行时环境，可以将一组动态语言服务添加到公共语言运行时 (CLR)  。 使用 DLR 可以轻松开发在 .NET Framework 上运行的动态语言，并为静态类型语言添加动态特征。

动态语言可以在运行时标识对象的类型，而在 C# 和 Visual Basic（使用 `Option Explicit On` 时）等静态类型的语言中，则必须在设计时指定对象类型。 动态语言包括 Lisp、Smalltalk、JavaScript、PHP、Ruby、Python、ColdFusion、Lua、Cobra 和 Groovy 等。

对开发人员而言，大部分动态语言具有以下优势：

- 能使用快速反馈循环（REPL 或称读取-评估-打印循环）。 用户可以输入多个语句，并立即执行这些语句查看结果。

- 同时支持自上而下的开发和传统的自下而上的开发。 例如，使用自上而下的方法时，可以调用尚未实现的函数，然后在需要时添加底层实现。

- 重构和代码修改变得更加简单，因为不必更改整个代码中的静态类型声明。

动态语言可以生成出色的脚本语言。 客户可以使用新命令和新功能轻松扩展使用动态语言创建的应用程序。 动态语言也经常用于创建网站和测试工具、维护服务器场、开发各种实用程序以及执行数据转换。

DLR 的目的是让动态语言系统可以在 .NET Framework 上运行，并为其提供 .NET 互操作性。 DLR 将动态对象添加到 C# 和 Visual Basic，以支持这些语言的动态行为，并实现这些语言与动态语言的互操作。

DLR 还可以帮助用户创建支持动态操作的库。 例如，如果用户的库使用的是 XML 或 JavaScript 对象表示法 (JSON) 对象，则对于使用 DLR 的语言，该对象可以显示为动态对象。 这样库用户可以编写语法更简单且更自然的代码来操作对象和访问对象成员。

例如，可以使用以下 C# 代码在 XML 中增加一个计数器。

`Scriptobj.SetProperty("Count", ((int)GetProperty("Count")) + 1);`

而使用 DLR 则可以改为使用以下代码实现相同的操作。

`scriptobj.Count += 1;`

DLR 与 CLR一样，也属于 .NET Framework，并随 .NET Framework 和 Visual Studio 安装包提供。 还可以从 GitHub 上的 [IronLanguages/dlr](https://github.com/IronLanguages/dlr) 存储库下载 DLR 的开源版本。

> [!NOTE]
> DLR 开源版本具有 Visual Studio 和 .NET Framework 中包含的 DLR 所具有的所有功能。 它还为语言实现者提供额外支持。 有关详细信息，请参阅位于 GitHub 上 [IronLanguages/dlr](https://github.com/IronLanguages/dlr) 存储库上的文档。

举例来说，使用 DLR 开发的语言包括以下语言：

- IronPython。 在 [GitHub](https://github.com/IronLanguages/ironpython2) 网站上作为开源软件提供。

- IronRuby。 在 [IronRuby](http://ironruby.net/) 网站上作为开源软件提供。

## <a name="primary-dlr-advantages"></a>DLR 的主要优点

 DLR 具有以下优点。

### <a name="simplifies-porting-dynamic-languages-to-the-net-framework"></a>简化了将动态语言移植到 .NET Framework 的操作

 DLR 让语言实现者无需创建词法分析器、解析器、语义分析器、代码生成器等过去需要自己创建的工具。 若要使用 DLR，语言需要生成以树形结构表示语言级代码的表达式树、运行时帮助程序例程以及用于实现 <xref:System.Dynamic.IDynamicMetaObjectProvider> 接口的可选动态对象  。 DLR 和 .NET Framework 自动执行大量代码分析和代码生成任务。 这样语言实现者可以集中处理独特的语言功能。

### <a name="enables-dynamic-features-in-statically-typed-languages"></a>在静态类型语言中实现动态功能

 现有的 .NET Framework 语言（如 C# 和 Visual Basic）可以创建动态对象，并将动态对象与静态类型对象结合使用。 例如，C# 和 Visual Basic 可将动态对象用于 HTML、文档对象模型 (DOM) 和 .NET 反射。

### <a name="provides-future-benefits-of-the-dlr-and-net-framework"></a>可以获得 DLR 和 .NET Framework 的后续改进

 使用 DLR 实现的语言可受益于 DLR 和 .NET Framework 的后续改进。 例如，如果 .NET Framework 发布的新版本改进了垃圾回收器或降低了程序集的加载时间，则使用 DLR 实现的语言可以立即受益于这些改进。 如果 DLR 添加了优化（例如更好的编译），那么使用 DLR 实现的所有语言的性能也会得到改善。

### <a name="enables-sharing-of-libraries-and-objects"></a>可以共享库和对象

 使用一种语言实现的对象和库可以用于其他语言。 DLR 还支持静态类型语言和动态语言之间的互操作。 例如，如果动态对象使用的库以动态语言编写，C# 也可以声明该对象。 同时，动态语言可以使用 .NET Framework 中的库。

### <a name="provides-fast-dynamic-dispatch-and-invocation"></a>可以快速动态调度和调用

 DLR 支持高级多态缓存，因此可以快速执行动态操作。 DLR 创建适用于绑定操作的规则，这些操作将对象用于必要的运行时实现，然后缓存这些规则，以避免在对相同类型的对象连续执行相同代码时进行大量消耗资源的绑定计算。

## <a name="dlr-architecture"></a>DLR 体系结构

 下图显示了动态语言运行时的体系结构。

 ![动态语言运行时体系结构概述](./media/dlr-archoverview.png "DLR_ArchOverview") DLR 体系结构

 DLR 向 CLR 添加了一组服务，以便更好地支持动态语言。 这些服务包括：

- 表达式树。 DLR 使用表达式树来表示语言语义。 为此，DLR 扩展了 LINQ 表达式树，使其包含控制流、分配和其他语言建模节点。 有关详细信息，请参阅[表达式树 (C#)](../../csharp/programming-guide/concepts/expression-trees/index.md) 或[表达式树 (Visual Basic)](../../visual-basic/programming-guide/concepts/expression-trees/index.md)。

- 调用站点缓存。 动态调用站点是代码中对动态对象执行 `a + b` 或 `a.b()` 等操作的位置  。 DLR 会缓存 `a` 和 `b`（通常为这些对象的类型）的特征，以及有关操作的信息。 如果之前已经执行过此类操作，则 DLR 会从缓存中检索所有的必要信息，以实现快速调度。

- 动态对象互操作性。 DLR 提供一组表示动态对象和操作的类和接口，供语言实现者和动态库的作者使用。 这些类和接口包括<xref:System.Dynamic.IDynamicMetaObjectProvider>、<xref:System.Dynamic.DynamicMetaObject>、<xref:System.Dynamic.DynamicObject> 和 <xref:System.Dynamic.ExpandoObject>。

DLR 在调用站点中使用联编程序不仅与 .NET Framework 进行通信，还与 Silverlight 和 COM 等其他基础结构和服务进行通信。 联编程序封装语言的语义，并指定如何使用表达式树在调用站点中执行操作。 这样使用 DLR 的动态和静态类型语言便可共享库，并访问 DLR 支持的所有技术。

## <a name="dlr-documentation"></a>DLR 文档

 如需深入了解如何使用 DLR 开源版本向语言添加动态行为，以及如何在 .NET Framework 中使用动态语言，请参阅 GitHub 上 [IronLanguages/dlr](https://github.com/IronLanguages/dlr/tree/master/Docs) 存储库上的文档。

## <a name="see-also"></a>请参阅

- <xref:System.Dynamic.ExpandoObject>
- <xref:System.Dynamic.DynamicObject>
- [公共语言运行时](../../standard/clr.md)
- [表达式树 (C#)](../../csharp/programming-guide/concepts/expression-trees/index.md)
- [表达式树 (Visual Basic)](../../visual-basic/programming-guide/concepts/expression-trees/index.md)
- [演练：创建和使用动态对象](../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
