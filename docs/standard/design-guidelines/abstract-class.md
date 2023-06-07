---
title: 抽象类设计
ms.date: 10/22/2008
helpviewer_keywords:
- type design guidelines, abstract classes
- abstract classes, design guidelines
- class library design guidelines [.NET Framework], classes
- classes [.NET Framework], abstract
- classes [.NET Framework], design guidelines
- type design guidelines, classes
ms.assetid: d3646e6d-5c1f-4922-8fb0-ec5effb30d60
ms.openlocfilehash: 6903e10c8695376d8ac5961461796c5413307f90
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94821640"
---
# <a name="abstract-class-design"></a>抽象类设计

❌ 不要在抽象类型中定义公共或受保护的内部构造函数。

 仅当用户需要创建类型的实例时，构造函数才应该是公共的。 由于不能创建抽象类型的实例，因此具有公共构造函数的抽象类型会错误地设计并且误导用户。

 ✔️在抽象类中定义受保护的或内部构造函数。

 受保护的构造函数更常见，只允许基类在创建子类型时进行自己的初始化。

 内部构造函数可用于将抽象类的具体实现限制为定义类的程序集。

 ✔️提供至少一种从你发运的每个抽象类继承的具体类型。

 这样做有助于验证抽象类的设计。 例如，  <xref:System.IO.FileStream?displayProperty=nameWithType> 是 <xref:System.IO.Stream?displayProperty=nameWithType> 抽象类的实现。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [类型设计准则](type.md)
- [框架设计准则](index.md)
