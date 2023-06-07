---
title: 命名参数
description: 了解参数命名准则。 例如，使用 & camel 大小写格式的描述性参数名称，& 考虑基于含义而不是类型进行命名。
ms.date: 10/22/2008
helpviewer_keywords:
- parameters, names
- names [.NET Framework], parameters
ms.assetid: ca3c956e-725a-441b-b4e3-eab5d472f41c
ms.openlocfilehash: 9f03eda511c2ef0c9565d270c52fd72bf54692d8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95698346"
---
# <a name="naming-parameters"></a>命名参数

除了可读性的明显原因外，请务必遵循有关参数名称的准则，因为当可视化设计工具提供 Intellisense 和类浏览功能时，参数将显示在文档和设计器中。

 ✔️在参数名称中使用 camelCasing。

 ✔️使用描述性参数名称。

 ✔️考虑使用基于参数含义而不是参数类型的名称。

### <a name="naming-operator-overload-parameters"></a>命名运算符重载参数

 `left` `right` 如果参数没有任何意义，✔️确实要使用和进行二元运算符重载参数名称。

 对于参数，✔️确实使用 `value` 一元运算符重载参数名称。

 ✔️考虑运算符重载参数有意义的名称，如果这样做会增加重要值。

 ❌ 不要对运算符重载参数名称使用缩写或数值索引。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [命名准则](naming-guidelines.md)
