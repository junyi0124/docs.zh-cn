---
title: 利用特性扩展元数据
description: 了解如何在 .NET 中使用特性扩展元数据。 特性是类似于关键字的描述性声明，用于批注编程元素，如类型和字段。
ms.date: 10/14/2020
helpviewer_keywords:
- metadata, extending
- attributes [.NET], metadata
- elements [.NET], attributes
- common language runtime, attributes
- annotating programming elements
- keyword-like descriptive declarations
- runtime, attributes
- extending metadata
ms.assetid: 30386922-1e00-4602-9ebf-526b271a8b87
ms.openlocfilehash: e41299b83ae451117ab6829eee928b0d2306be19
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829071"
---
# <a name="extend-metadata-using-attributes"></a>利用特性扩展元数据

公共语言运行时使你能够添加类似于关键字的描述性声明（称为特性），以便批注编程元素（如类型、字段、方法和属性）。 编译运行时的代码时，它将被转换为 Microsoft 中间语言 (MSIL)，并和编译器生成的元数据一起放置在可移植可执行 (PE) 文件内。 特性使你能够将额外的描述性信息放到可使用运行时反射服务提取的元数据中。 当你声明派生自 <xref:System.Attribute?displayProperty=nameWithType> 的特殊类的实例时，编译器会创建特性。

.NET 出于多种原因且为解决许多问题而使用特性。 特性描述如何将数据序列化、指定用于强制安全性的特征并限制通过实时 (JIT) 编译器进行优化，从而使代码易于调试。 特性还可记录文件的名称或代码的作者，或控制窗体开发过程中控件和成员的可见性。

## <a name="related-articles"></a>相关文章

|Title|描述|
|-----------|-----------------|
|[应用特性](applying-attributes.md)|描述如何将特性应用于代码的元素。|
|[编写自定义特性](writing-custom-attributes.md)|描述如何设计自定义特性类。|
|[检索存储在特性中的信息](retrieving-information-stored-in-attributes.md)|描述如何检索加载到执行上下文中的代码的自定义特性。|
|[元数据和自描述组件](../metadata-and-self-describing-components.md)|提供元数据的概述，并说明它是如何在 .NET 可移植可执行 (PE) 文件中实现的。|
|[如何：将程序集加载到仅反射上下文中](../../framework/reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)|说明如何检索仅反射上下文中的自定义特性信息。|

## <a name="reference"></a>参考

- <xref:System.Attribute?displayProperty=nameWithType>
