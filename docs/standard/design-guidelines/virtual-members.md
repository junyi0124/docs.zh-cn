---
title: 虚成员
ms.date: 10/22/2008
helpviewer_keywords:
- overridable members
- virtual members
- members [.NET Framework], virtual
ms.assetid: 8ff4eb97-0364-43ec-8a02-934b5cd94d19
ms.openlocfilehash: 7727ea7fcfdbbecf2e53ddd7a44b4cf907211f80
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734343"
---
# <a name="virtual-members"></a>虚成员

可以重写虚拟成员，从而更改子类的行为。 它们与回调在其提供的扩展性上非常相似，但它们在执行性能和内存消耗方面都更好。 此外，在需要创建一种特殊类型 (特殊化) 的方案中，虚拟成员更加自然。

 虚拟成员比回调和事件更好，但其性能不如非虚拟方法。

 虚拟成员的主要缺点是，在编译时只能修改虚拟成员的行为。 可以在运行时修改回调行为。

 由于对虚拟成员的任何调用都可以以不可预知的方式重写，并且可以执行任意代码，因此虚拟成员（如回调 (和回调) 可能更多的设计、测试和维护。 此外，通常还需要更多的工作来清楚地定义虚拟成员的协定，因此设计和记录这些成员的成本更高。

 ❌ 不要将成员设为虚拟的，除非您有充分的理由这样做，并且您知道与设计、测试和维护虚拟成员相关的所有成本。

 在无需中断兼容性的情况下，虚拟成员就可以对其进行更改，而不是包容性。 而且，它们比非虚拟成员慢，主要是因为对虚拟成员的调用不内联。

 ✔️考虑将扩展性限制为仅绝对必要的。

 ✔️比虚拟成员的公共可访问性更喜欢受保护的可访问性。 公共成员应通过调用受保护的虚拟成员) 来提供扩展性 (。

 类的公共成员应为该类的直接使用者提供正确的一组功能。 虚拟成员设计为在子类中被重写，受保护的可访问性是将所有虚拟扩展点的范围限定为可使用这些扩展点的最佳方式。

 *部分 &copy; 2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [扩展性设计](designing-for-extensibility.md)
