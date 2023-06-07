---
title: 类型设计准则
ms.date: 10/22/2008
helpviewer_keywords:
- type design guidelines
- type design guidelines, about type design guidelines
- class library design guidelines [.NET Framework], type design guidelines
- types [.NET Framework], design guidelines
ms.assetid: 6b49314e-8bba-43ea-97ca-4e0255812f95
ms.openlocfilehash: a20744f76433ff12456967e4d41d9a13b6f5d46c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677526"
---
# <a name="type-design-guidelines"></a>类型设计准则

从 CLR 的角度来看，只有两类类型：引用类型和值类型，但对于框架设计的讨论，我们将类型分成多个逻辑组，每个逻辑组都有自己的特定设计规则。

 类是引用类型的一般情况。 它们构成大多数框架中的类型。 类在其支持的丰富、面向对象的功能集方面欠其普及。 基类和抽象类是与扩展性相关的特殊逻辑组。

 接口是可以由引用类型和值类型实现的类型。 因此，它们可以充当引用类型和值类型的多态层次结构的根。 此外，接口还可用于模拟多个继承，而 CLR 并不本机支持此方法。

 结构是值类型的一般事例，应为类似于语言基元的小型简单类型保留。

 枚举是值类型的一种特殊情况，用于定义短值集，如一周中的几天、控制台颜色等。

 静态类是用于静态成员的容器的类型。 它们通常用于提供其他操作的快捷方式。

 委托、异常、特性、数组和集合都是专用于特定用途的引用类型的特殊情况，本指南中的其他地方讨论了用于设计和使用的准则。

 ✔️确保每个类型都是一组定义完善的相关成员，而不只是一个随机的无关功能集合。

## <a name="in-this-section"></a>本节内容

 [在类和结构之间选择](choosing-between-class-and-struct.md)[抽象类设计](abstract-class.md)[静态类设计](static-class.md)[接口设计](interface.md)[结构设计](struct.md)[枚举设计](enum.md)[嵌套类型](nested-types.md)*部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
