---
title: 密封
ms.date: 10/22/2008
helpviewer_keywords:
- limiting extensibility
- classes [.NET Framework], sealing
- preventing customization
- sealed classes
ms.assetid: cc42267f-bb7a-427a-845e-df97408528d4
ms.openlocfilehash: bf8aad5d79e659ad9a767c2b0992eb9ee05fd531
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730937"
---
# <a name="sealing"></a>密封

面向对象的框架的一项功能是，开发人员可以采用框架设计器无法预料的方式对其进行扩展和自定义。 这是可扩展设计的强大功能和危险。 当你设计框架时，这一点非常重要，需要在需要时认真设计扩展性，并在有风险时限制扩展性。

 阻止扩展性的强大机制是密封的。 可以密封类或单个成员。 密封类可防止用户从类继承。 密封成员可防止用户重写特定成员。

 ❌ 不要密封类，无需采取充分的理由。

 密封类，因为您不能认为扩展性方案不是一个好的原因。 出于各种 nonobvious 原因（如添加便利成员），框架用户喜欢从类继承。 有关用户要从类型继承的 nonobvious 原因的示例，请参阅未 [密封类](unsealed-classes.md) 。

 密封类的合理原因包括：

- 类是静态类。 请参阅 [静态类设计](static-class.md)。

- 类在继承的受保护成员中存储安全敏感机密。

- 类继承多个虚拟成员，并分别对它们进行密封的开销超过了使类不密封的好处。

- 类是需要非常快速的运行时查找的属性。 密封属性比非密封属性略高。 请参阅 [特性](attributes.md)。

 ❌ 不要在密封类型中声明受保护成员或虚拟成员。

 按照定义，不能从继承密封类型。 这意味着不能在密封类型上调用受保护成员，并且不能重写密封类型上的虚方法。

 ✔️考虑密封重写的成员。

 引入虚拟成员时可能产生的问题 (在 [虚拟成员](virtual-members.md) 中进行了介绍) 应用于重写，尽管稍微略低一些。 密封替代会阻止从继承层次结构中的该点开始的这些问题。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [扩展性设计](designing-for-extensibility.md)
- [未密封类](unsealed-classes.md)
