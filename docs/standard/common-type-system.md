---
title: 常规类型系统和公共语言规范
description: 了解通用类型系统 (CTS) 和公共语言规范 (CLS) 如何使 .NET 可支持多种语言。
ms.date: 06/20/2016
ms.assetid: 3b1f5725-ac94-4f17-8e5f-244442438a4d
ms.openlocfilehash: e60205450e2f156407deb7be6b9c497d090b6f7b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822875"
---
# <a name="common-type-system--common-language-specification"></a>常规类型系统和公共语言规范

再次介绍 .NET 领域内大量使用的两个术语，对于理解 .NET 实现如何能够允许多语言开发及其工作原理，这两个术语至关重要。

## <a name="common-type-system"></a>常规类型系统

首先，请记住 .NET 实现对语言不可知。 这不仅意味着程序员可以使用可编译为 IL 的任意语言编写代码。 还意味着程序员需要能够与使用 .NET 实现上可用的其他语言编写的代码进行交互。

为透明地执行此操作，必须使用某种通用方式描所有受支持类型。 这正是通用类型系统 (CTS) 的职责。 其功能如下：

* 建立用于跨语言执行的框架。
* 提供面向对象的模型，支持在 .NET 实现上实现各种语言。
* 定义处理类型时所有语言都必须遵守的一组规则。
* 提供包含应用程序开发中使用的基本基元数据类型（如 `Boolean`、`Byte`、`Char` 等）的库。

CTS 将定义应支持的两种主要类型：引用和值类型。 其名称表明了其定义。

引用类型对象由对对象实际值的引用表示；此处的引用与 C/C++ 中的指针类似。 它仅引用对象值所在的内存位置。 这对这些类型的用法有重大影响。 例如，如果将引用类型分配给个变量，然后将该变量传递到方法，则对该对象的任意更改都将反映到主对象上；不涉及复制操作。

值类型则相反，其中对象由其值表示。 如果将值类型分配给变量，实质上就是复制对象的值。

CTS 将定义多个类型类别，每个类别均有其特定的语义和用法：

* 类
* 结构
* 枚举
* 界面
* 委托

CTS 还将定义类型的所有其他属性，例如访问修饰符、有效的类型成员以及继承和重载的工作原理等。 遗憾的是，有关上述任意一点的深入介绍已超出此类介绍性文章的介绍范围，但可以参阅底部的[更多资源](#more-resources)部分，获取包含这些主题的详细内容的链接。

## <a name="common-language-specification"></a>公共语言规范

为实现完全互操作性情景，代码中创建的所有对象都必须依赖于使用它的语言（即其调用方）的某些共性。 由于存在多种不同语言，因此 .NET 在公共语言规范 (CLS) 中指定了这些共性。 CLS 定义了许多常见应用程序所需的一组功能。 对于在 .NET 上实现的语言，它还就语言需要支持的内容提供了一组脚本。

CLS 是 CTS 的子集。 这意味着，CTS 中的所有规则也适用于 CLS，除非 CLS 规则更严格。 如果仅使用 CLS 中的规则生成组件（即在其 API 中仅公开 CLS 功能），则将该组件视为 **符合 CLS**。 例如，`<framework-librares>` 完全符合 CLS，因为它们需要对 .NET 支持的所有语言有效。

可参阅下方[更多资源](#more-resources)部分中的文档，大致了解 CLS 中的所有功能。

## <a name="more-resources"></a>更多资源

* [常规类型系统](./base-types/common-type-system.md)
* [公共语言规范](language-independence-and-language-independent-components.md)
