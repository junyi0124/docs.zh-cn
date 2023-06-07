---
title: 字段设计
ms.date: 10/22/2008
helpviewer_keywords:
- fields, design guidelines
- read-only fields
- member design guidelines, fields
ms.assetid: 7cb4b0f3-7a10-4c93-b84d-733f7134fcf8
ms.openlocfilehash: 4329ca9e74fa02411bd39755a432fc7d4858597d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706692"
---
# <a name="field-design"></a>字段设计

封装原则是面向对象的设计中最重要的概念之一。 此原则表明，存储在对象中的数据应只能访问该对象。

 解释此原则的一种有用方法是说，一种类型应设计为，以便对该类型的字段进行更改 (名称或类型) 更改，而不会中断类型成员的代码。 此解释会立即表示所有字段都必须是私有的。

 我们从此严格限制中排除了常量和静态只读字段，因为此类字段（几乎按定义）从不需要更改。

 ❌ 不要提供公共或受保护的实例字段。

 应提供用于访问字段的属性，而不是将其设为公共或受保护。

 ✔️对永远不会更改的常量使用常量字段。

 编译器会直接将常量字段的值直接引入到调用代码。 因此，可能永远不会更改常量值，而不会带来中断兼容性的风险。

 ✔️ `readonly` 对预定义对象实例使用公共静态字段。

 如果有类型的预定义实例，请将它们声明为类型本身的公共只读静态字段。

 ❌ 不要将可变类型的实例分配给 `readonly` 字段。

 可变类型是具有实例的类型，这些实例可在实例化后进行修改。 例如，数组、大多数集合和流都是可变类型，但 <xref:System.Int32?displayProperty=nameWithType> 、 <xref:System.Uri?displayProperty=nameWithType> 和 <xref:System.String?displayProperty=nameWithType> 都是不可变的。 引用类型字段上的只读修饰符可防止替换存储在字段中的实例，但它不会通过调用更改实例的成员来防止修改字段的实例数据。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
