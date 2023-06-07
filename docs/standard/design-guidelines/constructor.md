---
title: 构造函数设计
ms.date: 10/22/2008
helpviewer_keywords:
- member design guidelines, constructors
- constructors, design guidelines
- instance constructors
- type constructors
- virtual members
- constructors, types
- parameterless constructors
- static constructors
ms.assetid: b4496afe-5fa7-4bb0-85ca-70b0ef21e6fc
ms.openlocfilehash: 27fb73aa01adf31117d1b82724873db3a03fd269
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94821393"
---
# <a name="constructor-design"></a>构造函数设计

有两种类型的构造函数：类型构造函数和实例构造函数。

类型构造函数是静态的，在使用类型之前由 CLR 运行。 实例构造函数在创建类型的实例时运行。

类型构造函数不能采用任何参数。 实例构造函数可以。 不采用任何参数的实例构造函数通常称为无参数的构造函数。

构造函数是创建类型的实例的最自然方式。 大多数开发人员会搜索并尝试使用构造函数，然后再考虑创建实例 (如工厂方法) 的替代方法。

✔️考虑提供简单、理想的默认构造函数。

简单的构造函数包含非常少的参数，所有参数均为基元或枚举。 此类简单构造函数会提高框架的可用性。

如果所需操作的语义不会直接映射到新实例的构造，或者如果遵循构造函数设计准则非自然，则✔️考虑使用静态工厂方法而不是构造函数。

✔️使用构造函数参数作为设置主属性的快捷方式。

使用空的构造函数后跟某些属性集和使用具有多个参数的构造函数之间的语义不应存在任何差异。

如果构造函数参数仅用于设置属性，则✔️为构造函数参数和属性使用相同的名称。

此类参数和属性的唯一区别在于大小写形式。

✔️在构造函数中执行最少工作量。

除捕获构造函数参数外，构造函数不应执行太多工作。 任何其他处理的成本应延迟到必需的时间。

如果适合，✔️将从实例构造函数引发异常。

如果需要此类构造函数，✔️会在类中显式声明公共无参数构造函数。

如果没有显式声明类型上的任何构造函数，则许多语言 (如 c # ) 将自动添加一个公共的无参数构造函数。  (抽象类获取受保护的构造函数。 ) 

向类添加参数化构造函数会阻止编译器添加无参数的构造函数。 这通常会导致意外的重大更改。

❌ 避免在结构上显式定义无参数的构造函数。

这样可以更快地创建数组，因为如果没有定义无参数的构造函数，则不必在数组中的每个槽上运行它。 请注意，许多编译器（包括 c #）不允许结构出于此原因而提供无参数的构造函数。

❌ 避免在其构造函数中的对象上调用虚拟成员。

调用虚拟成员将导致调用最常派生的重写，即使派生程度最高的构造函数尚未完全运行也是如此。

## <a name="type-constructor-guidelines"></a>类型构造函数指南

✔️使静态构造函数成为私有构造函数。

静态构造函数（也称为类构造函数）用于初始化类型。 CLR 在创建该类型的第一个实例或调用该类型的任何静态成员之前调用静态构造函数。 用户无法控制何时调用静态构造函数。 如果静态构造函数不是私有，则它可由 CLR 以外的代码调用。 根据构造函数中执行的操作，这可能导致意外的行为。 C # 编译器强制静态构造函数是私有的。

❌ 不要从静态构造函数引发异常。

如果从类型构造函数引发异常，则类型在当前应用程序域中不可用。

✔️考虑以内联方式初始化静态字段，而不是显式使用静态构造函数，因为运行时能够优化没有显式定义的静态构造函数的类型的性能。

*部分©2005，2009 Microsoft Corporation。保留所有权利。*

*经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
