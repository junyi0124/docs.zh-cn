---
title: 嵌套类型
ms.date: 10/22/2008
helpviewer_keywords:
- types, nested
- public nested types
- type design guidelines, nested types
- nested types
- members [.NET Framework], type
- class library design guidelines [.NET Framework], nested types
ms.assetid: 12feb7f0-b793-4d96-b090-42d6473bab8c
ms.openlocfilehash: bc0aee32b5cc1d40afdd9cce8260d5b5341a687d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706393"
---
# <a name="nested-types"></a>嵌套类型

嵌套类型是在另一种类型的作用域内定义的类型，称为封闭类型。 嵌套类型可以访问其封闭类型的所有成员。 例如，它可以访问在封闭类型中定义的私有字段，还可以访问在封闭类型的所有祖先中定义的受保护字段。

 一般而言，应慎用嵌套类型。 其原因有若干： 有些开发人员并不完全熟悉该概念。 例如，这些开发人员可能遇到声明嵌套类型变量的语法问题。 嵌套类型也与它们的封闭类型紧密耦合，因此不适合用作通用类型。

 嵌套类型最适合用于建模其封闭类型的实现细节。 最终用户极少需要声明嵌套类型的变量，几乎不应显式实例化嵌套类型。 例如，集合的枚举器可以是该集合的嵌套类型。 枚举器通常通过其封闭类型进行实例化，由于许多语言支持 foreach 语句，因此枚举器变量很少需要由最终用户声明。

 如果嵌套类型与其外部类型之间的关系是需要成员可访问性语义，✔️确实使用嵌套类型。

 ❌ 不要使用公共嵌套类型作为逻辑分组构造;为此使用命名空间。

 ❌ 避免公开公开的嵌套类型。 唯一的例外是，只需在很少的情况下（如子类化或其他高级自定义方案）声明嵌套类型的变量。

 ❌ 如果类型可能在包含类型的外部引用，请不要使用嵌套类型。

 例如，传递给在类上定义的方法的枚举不应定义为类中的嵌套类型。

 ❌ 如果嵌套类型需要通过客户端代码进行实例化，请不要使用嵌套类型。  如果某个类型具有公共构造函数，则它可能不会嵌套。

 如果可实例化类型，则似乎指示该类型在框架中具有一个位置，你可以创建该类型，使用该类型，并在不使用外部类型的情况下销毁该类型 () ，因此不应进行嵌套。 不应将内部类型广泛地在外部类型的外部重用，无需任何与外部类型的关系。

 ❌ 不要将嵌套类型定义为接口的成员。 许多语言不支持这种构造。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [类型设计准则](type.md)
- [框架设计准则](index.md)
