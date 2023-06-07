---
title: 相等运算符
ms.date: 10/22/2008
helpviewer_keywords:
- class library design guidelines [.NET Framework], Equals method
- class library design guidelines [.NET Framework], equality operator
- equality operator (==) [.NET Framework]
- Equals method
- == operator (equality) [.NET Framework]
ms.assetid: bc496a91-fefb-4ce0-ab4c-61f09964119a
ms.openlocfilehash: 2331a852adb4dd254af85060a5077f454bcfe0eb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734421"
---
# <a name="equality-operators"></a>相等运算符

本节讨论重载相等运算符并将 `operator==` 和 `operator!=` 作为相等运算符引用。

 ❌ 不要重载某个相等运算符而不是另一个相等运算符。

 ✔️确保 <xref:System.Object.Equals%2A?displayProperty=nameWithType> 和相等运算符具有完全相同的语义和类似的性能特征。

 这通常意味着在 `Object.Equals` 重载相等运算符时需要重写。

 ❌ 避免从相等运算符引发异常。

 例如，如果其中一个参数为 null 而不是引发，则返回 false `NullReferenceException` 。

## <a name="equality-operators-on-value-types"></a>值类型上的相等运算符

 ✔️如果相等性是有意义的，则对值类型重载相等运算符。

 在大多数编程语言中，值类型没有的默认实现 `operator==` 。

## <a name="equality-operators-on-reference-types"></a>引用类型上的相等运算符

 ❌ 避免对可变引用类型重载相等运算符。

 许多语言具有用于引用类型的内置相等运算符。 内置运算符通常实现引用相等性，而当默认行为更改为值相等性时，很多开发人员都很惊讶。

 对于不可变的引用类型，此问题将得到缓解，因为不可变性使发现引用相等性和值相等性之间的差异变得更加困难。

 ❌ 如果实现的速度远远低于引用相等性，请避免对引用类型重载相等运算符。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [使用准则](usage-guidelines.md)
