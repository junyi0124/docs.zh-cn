---
title: 结构设计
ms.date: 10/22/2008
helpviewer_keywords:
- class library design guidelines [.NET Framework], structures
- deallocating structures
- allocating structures
- value types, structures
- structure design
- type design guidelines, structures
- structures [.NET Framework], design guidelines
ms.assetid: 1f48b2d8-608c-4be6-9ba4-d8f203ed9f9f
ms.openlocfilehash: 7bb7e63004df2eb7541ba8d4f1118ea2272db126
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730898"
---
# <a name="struct-design"></a>结构设计

一般用途值类型最常称为结构，即其 c # 关键字。 本部分提供常规结构设计的准则。

 ❌ 不要为结构提供无参数的构造函数。

 遵循此准则后，便可以创建结构的数组，而不必在每个数组项上运行构造函数。 请注意，c # 不允许结构具有无参数的构造函数。

 ❌ 不要定义可变值类型。

 可变值类型有几个问题。 例如，当属性 getter 返回值类型时，调用方会收到一个副本。 由于副本是隐式创建的，因此开发人员可能不会注意到它们正在改变副本，而不是原始值。 此外，某些语言 (动态语言，特别) 使用可变值类型时存在问题，因为在取消引用后，即使是本地变量，也会导致复制。

 ✔️确保将所有实例数据的状态设置为零、false 或 null (适当的) 有效。

 这可以防止在创建结构数组时意外创建无效的实例。

 ✔️ <xref:System.IEquatable%601> 在值类型上实现。

 <xref:System.Object.Equals%2A?displayProperty=nameWithType>值类型上的方法导致装箱，其默认实现并不太有效，因为它使用反射。 <xref:System.IEquatable%601.Equals%2A> 可以具有更好的性能，并且可以实现，以便它不会导致装箱。

 ❌ 不要显式扩展 <xref:System.ValueType> 。 事实上，大多数语言都禁止这样做。

 通常情况下，结构可能非常有用，但只应用于不会频繁装箱的小型单个不可变值。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [类型设计准则](type.md)
- [框架设计准则](index.md)
- [在类和结构之间选择](choosing-between-class-and-struct.md)
