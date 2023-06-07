---
title: 用于实现抽象的基类
ms.date: 10/22/2008
helpviewer_keywords:
- abstractions [.NET Framework]
- base classes, abstractions
ms.assetid: 37a2d9a4-9721-482a-a40f-eee2c1d97875
ms.openlocfilehash: 9e49b79609a2d16a79a80727295d53bb36ec5943
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701310"
---
# <a name="base-classes-for-implementing-abstractions"></a>用于实现抽象的基类

严格地说，当另一个类派生类时，该类将成为基类。 不过，在本部分中，基类是设计用于提供公共抽象的类，或用于其他类的类，通过继承来重用某些默认实现。 基类通常位于继承层次结构中，在层次结构的根下的抽象之间，还有几个自定义实现。

 它们充当实现抽象的实现帮助程序。 例如，用于项的有序集合的某个框架抽象是 <xref:System.Collections.Generic.IList%601> 接口。 实现并 <xref:System.Collections.Generic.IList%601> 不重要，因此，该框架提供了多个基类，如 <xref:System.Collections.ObjectModel.Collection%601> 和 <xref:System.Collections.ObjectModel.KeyedCollection%602> ，它们用作实现自定义集合的帮助器。

 基类通常不适合用作抽象，因为它们往往包含太多的实现。 例如， `Collection<T>` 基类包含许多与实现非泛型接口的事实相关的实现 `IList` (以便更好地集成非泛型) 集合，而不是在它的一个字段中存储在内存中的项的集合。

 正如前面所讨论的，基类可为需要实现抽象的用户提供宝贵的帮助，但同时也会有重大责任。 它们添加了表面区域并增加了继承层次结构的深度，因此在概念上使框架复杂化。 因此，仅当基类为框架用户提供重要值时，才应使用它们。 如果用户仅向框架的实施者提供值，则应避免使用这些值，在这种情况下，应强烈考虑委托给内部实现，而不是基类继承。

 ✔️考虑使基类抽象，即使它们不包含任何抽象成员也是如此。 这就清楚地与用户沟通，该类专门用于从继承。

 ✔️考虑将基类置于不同于主线方案类型的命名空间中。 按照定义，基类适用于高级扩展性方案，因此对于大多数用户来说并不重要。

 ❌ 如果类应在公共 Api 中使用，请避免使用 "Base" 后缀命名基类。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [扩展性设计](designing-for-extensibility.md)
