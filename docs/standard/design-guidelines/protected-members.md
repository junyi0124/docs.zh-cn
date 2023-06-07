---
title: 受保护的成员
ms.date: 10/22/2008
helpviewer_keywords:
- members [.NET Framework], protected
- protected members
- classes [.NET Framework], unsealed
- classes [.NET Framework], protected members
- unsealed classes
- customizing class behavior
ms.assetid: aa0b58ee-3956-494d-ab48-471ae5db8740
ms.openlocfilehash: a6f36ac4f994fdc3211cac619cc0b20f7b0335b1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730950"
---
# <a name="protected-members"></a>受保护的成员

受保护的成员不提供任何可扩展性，但它们可以通过创建更强大的子类进行扩展。 它们可用于公开高级自定义项选项，而不会使主公共接口复杂化。

 框架设计人员需要小心处理受保护的成员，因为名称 "protected" 可以提供虚假的安全性理解。 任何人都可以为未密封的类提供子类并访问受保护的成员，因此用于公共成员的所有相同的防御性编码方法都适用于受保护的成员。

 ✔️考虑使用受保护的成员进行高级自定义。

 ✔️将未密封类中的受保护成员视为公共的，目的是为了实现安全、文档和兼容性分析。

 任何人都可以从类继承并访问受保护的成员。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [扩展性设计](designing-for-extensibility.md)
