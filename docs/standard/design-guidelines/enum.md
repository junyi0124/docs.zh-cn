---
title: 枚举设计
description: 枚举的设计，这是一种特殊类型的值类型。 简单枚举保留小型的已关闭选择集。 标记枚举支持对枚举值执行按位运算。
ms.date: 10/22/2008
helpviewer_keywords:
- type design guidelines, enumerations
- simple enumerations
- enumerations [.NET Framework], design guidelines
- class library design guidelines [.NET Framework], enumerations
- flags enumerations
ms.assetid: dd53c952-9d9a-4736-86ff-9540e815d545
ms.openlocfilehash: a2e19197b114daa2a0956a6fc87231a6a81de916
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94821354"
---
# <a name="enum-design"></a>枚举设计

枚举是一种特殊类型的值类型。 有两种类型的枚举：简单枚举和标记枚举。

简单枚举表示小型关闭的选项集。 简单枚举的一个常见示例是一组颜色。

标记枚举旨在支持枚举值的按位运算。 标志枚举的一个常见示例是选项的列表。

✔️确实使用枚举来表示值集的强类型参数、属性和返回值。

✔️使用枚举而不是静态常量。

❌ 请勿对开放集使用枚举 (如操作系统版本、朋友名称等 ) 。

❌ 不要提供旨在供将来使用的保留枚举值。

你始终可以在以后的阶段将值添加到现有枚举。 有关向枚举添加值的详细信息，请参阅 [将值添加到枚举](#add_value) 。 保留值只是污染一组真实值，往往导致用户错误。

❌ 避免公开只包含一个值的枚举。

若要确保未来的 C Api 可扩展性，常见的做法是将保留参数添加到方法签名。 此类保留参数可表示为具有单个默认值的枚举。 不应在托管 Api 中完成此操作。 方法重载允许在未来版本中添加参数。

❌ 不要在枚举中包含 sentinel 值。

尽管它们有时对框架开发人员很有帮助，但对于框架的用户来说，sentinel 值会令人感到困惑。 它们用于跟踪枚举的状态，而不是由枚举表示的集中的值之一。

✔️在简单枚举上提供零值。

请考虑调用值，如 "None"。 如果此类值不适合于此特定枚举，则应将基础值指定为零的最常见默认值。

✔️考虑 <xref:System.Int32> 在大多数编程语言中使用 (默认值) 作为枚举的基础类型，除非满足以下任一条件：

- 枚举是一个标志枚举，你有超过32个标志，或者预计将来会有更多的标志。

- 基础类型需要不同于 <xref:System.Int32> ，与需要不同大小枚举的非托管代码的互操作性更简单。

- 较小的基础类型会显著节省空间。 如果希望枚举主要作为控制流的参数使用，则大小没有差别。 如果存在以下情况，大小节省可能会很大：

  - 希望枚举在非常频繁实例化的结构或类中用作字段。

  - 您希望用户创建枚举实例的大型数组或集合。

  - 需要序列化大量枚举实例。

对于内存中使用，请注意，托管对象始终是 `DWORD` 一致的，因此，你可以在一个实例中有效地使用多个枚举或其他小型结构将一个较小的枚举打包，以便进行差别，因为总实例大小始终会向上舍入为 `DWORD` 。

✔️用名词或名词短语复数和简单枚举作为名词或名词短语来命名标志枚举。

❌ 不要直接扩展 <xref:System.Enum?displayProperty=nameWithType> 。

<xref:System.Enum?displayProperty=nameWithType> 是 CLR 用来创建用户定义的枚举的特殊类型。 大多数编程语言都提供了一个编程元素，该元素可让你访问此功能。 例如，在 c # 中， `enum` 关键字用于定义枚举。

<a name="design"></a>

### <a name="designing-flag-enums"></a>设计标志枚举

✔️应用 <xref:System.FlagsAttribute?displayProperty=nameWithType> 来标记枚举。 不要将此属性应用于简单枚举。

✔️对标志枚举值使用2的幂，以便可以使用按位 "或" 运算自由合并这些值。

✔️考虑为常用的标志组合提供特殊的枚举值。

按位运算是一个高级概念，对于简单任务，不应是必需的。 <xref:System.IO.FileAccess.ReadWrite> 这是一个特殊值的示例。

❌ 避免创建标志枚举，其中某些值的组合无效。

❌ 避免使用值为零的标记枚举值，除非该值表示 "所有标志均已清除" 并正确命名，如下一个准则所述。

✔️ DO name 标记枚举的零值 `None` 。 对于标志枚举，值始终为 "清除所有标志"。

<a name="add_value"></a>

### <a name="adding-value-to-enums"></a>向枚举添加值

很常见的情况是，在已交付枚举后，需要向其添加值。 在从现有 API 返回新添加的值时存在潜在的应用程序兼容性问题，因为编写不当的应用程序可能无法正确处理新值。

✔️考虑向枚举添加值，而不考虑较小的兼容性风险。

如果对枚举的添加操作导致应用程序不兼容，请考虑添加一个新的 API，该 API 将返回新的和旧的值，并弃用旧的 API，该 API 应继续只返回旧值。 这将确保现有的应用程序保持兼容。

*部分©2005，2009 Microsoft Corporation。保留所有权利。*

*经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [类型设计准则](type.md)
- [框架设计准则](index.md)
