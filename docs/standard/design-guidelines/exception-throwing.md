---
title: 异常引发
ms.date: 10/22/2008
helpviewer_keywords:
- exceptions, throwing
- explicitly throwing exceptions
- throwing exceptions, design guidelines
ms.assetid: 5388e02b-52f5-460e-a2b5-eeafe60eeebe
ms.openlocfilehash: d41467b971e43ca9b22c59e3b64bdd45d16c740b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734395"
---
# <a name="exception-throwing"></a>异常引发

本部分中所述的异常引发准则需要一个良好的定义来表示执行失败。 每当成员无法执行其设计时所用的操作时，将发生执行失败 (成员名称表示) 。 例如，如果该 `OpenFile` 方法无法向调用方返回打开的文件句柄，则会将其视为执行失败。

 大多数开发人员对使用错误（例如除以零或空引用）的使用异常感到非常熟悉。 在框架中，异常用于所有错误情况，包括执行错误。

 ❌ 不返回错误代码。

 异常是在框架中报告错误的主要方法。

 ✔️通过引发异常来报告执行失败。

 ✔️考虑通过调用 `System.Environment.FailFast` ( .NET Framework 2.0 功能) 终止进程，而不是在代码遇到不安全的情况下引发异常，以便进一步执行。

 ❌ 如果可能，请不要为正常控制流使用异常。

 除了可能出现争用条件的系统故障和操作，框架设计器还应设计 Api，使用户能够编写不引发异常的代码。 例如，你可以提供一种在调用成员之前检查前置条件的方法，以便用户可以编写不引发异常的代码。

 用于检查另一个成员的前置条件的成员通常称为测试人员，实际执行该工作的成员称为 doer。

 在某些情况下，Tester-Doer 模式可能会产生不可接受的性能开销。 在这种情况下，应考虑所谓的 Try-Parse 模式 (有关) 详细信息，请参阅 [异常和性能](exceptions-and-performance.md) 。

 ✔️考虑引发异常的性能影响。 每秒以上100的引发率可能会显著影响大多数应用程序的性能。

 ✔️会记录公共可调用成员引发的所有异常，因为违反了成员协定 (而不是系统故障) ，并将其视为协定的一部分。

 作为协定一部分的异常不应从一个版本更改为下一个 (即，异常类型不应更改，且不应将新异常添加) 。

 ❌ 不具有可引发的公共成员或不基于某个选项的公共成员。

 ❌ 不具有返回异常作为返回值或参数的公共成员 `out` 。

 从公共 Api 返回异常，而不是引发异常，这会破坏基于异常的错误报告的许多好处。

 ✔️考虑使用异常生成器方法。

 通常会从不同的位置引发相同的异常。 若要避免代码膨胀，请使用创建异常并初始化其属性的帮助器方法。

 而且，引发异常的成员不能内联。 在生成器中移动 throw 语句可能会允许该成员内联。

 ❌ 不引发异常筛选器块中的异常。

 当异常筛选器引发异常时，CLR 将捕获该异常，并且筛选器将返回 false。 此行为与执行和显式返回 false 的筛选器不区分，因此很难进行调试。

 ❌ 避免从 finally 块显式引发异常。 调用引发的方法可接受隐式引发的异常。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [异常设计准则](exceptions.md)
