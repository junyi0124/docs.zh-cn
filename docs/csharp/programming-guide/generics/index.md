---
title: 泛型 - C# 编程指南
description: 了解泛型。 泛型类型可以最大程度地提高代码重用、类型安全性和性能，通常用于创建集合类。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generics
- generics [C#]
ms.assetid: 75ea8509-a4ea-4e7a-a2b3-cf72482e9282
ms.openlocfilehash: beef9c20e3ac62505bc7a4584b404637935de1dc
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87299391"
---
# <a name="generics-c-programming-guide"></a>泛型（C# 编程指南）

泛型将类型参数的概念引入 .NET，这样就可设计具有以下特征的类和方法：在客户端代码声明并初始化这些类或方法之前，这些类或方法会延迟指定一个或多个类型。 例如，通过使用泛型类型参数 `T`，可以编写其他客户端代码能够使用的单个类，而不会产生运行时转换或装箱操作的成本或风险，如下所示：

[!code-csharp[csProgGuideGenerics#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#1)]

泛型类和泛型方法兼具可重用性、类型安全性和效率，这是非泛型类和非泛型方法无法实现的。 泛型通常与集合以及作用于集合的方法一起使用。 <xref:System.Collections.Generic> 命名空间包含几个基于泛型的集合类。 非泛型集合（如 <xref:System.Collections.ArrayList>）不建议使用，并且保留用于兼容性目的。 有关详细信息，请参阅 [.NET 中的泛型](../../../standard/generics/index.md)。

当然，也可以创建自定义泛型类型和泛型方法，以提供自己的通用解决方案，设计类型安全的高效模式。 以下代码示例演示了出于演示目的的简单泛型链接列表类。 （大多数情况下，应使用 .NET 提供的 <xref:System.Collections.Generic.List%601> 类，而不是自行创建类。）在通常使用具体类型来指示列表中所存储项的类型的情况下，可使用类型参数 `T`。 其使用方法如下：

- 在 `AddHead` 方法中作为方法参数的类型。
- 在 `Node` 嵌套类中作为 `Data` 属性的返回类型。
- 在嵌套类中作为私有成员 `data` 的类型。

 请注意，`T` 可用于 `Node` 嵌套类。 如果使用具体类型实例化 `GenericList<T>`（例如，作为 `GenericList<int>`），则出现的所有 `T` 都将替换为 `int`。

[!code-csharp[csProgGuideGenerics#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#2)]

以下代码示例演示了客户端代码如何使用泛型 `GenericList<T>` 类来创建整数列表。 只需更改类型参数，即可轻松修改以下代码，创建字符串或任何其他自定义类型的列表：

[!code-csharp[csProgGuideGenerics#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#3)]

## <a name="generics-overview"></a>泛型概述

- 使用泛型类型可以最大限度地重用代码、保护类型安全性以及提高性能。
- 泛型最常见的用途是创建集合类。
- .NET 类库在 <xref:System.Collections.Generic> 命名空间中包含几个新的泛型集合类。 应尽可能使用这些类来代替某些类，如 <xref:System.Collections> 命名空间中的 <xref:System.Collections.ArrayList>。
- 可以创建自己的泛型接口、泛型类、泛型方法、泛型事件和泛型委托。
- 可以对泛型类进行约束以访问特定数据类型的方法。
- 在泛型数据类型中所用类型的信息可在运行时通过使用反射来获取。

## <a name="related-sections"></a>相关章节

- [泛型类型参数](generic-type-parameters.md)
- [类型参数的约束](constraints-on-type-parameters.md)
- [泛型类](generic-classes.md)
- [泛型接口](generic-interfaces.md)
- [泛型方法](generic-methods.md)
- [泛型委托](generic-delegates.md)
- [C++ 模板和 C# 泛型之间的区别](differences-between-cpp-templates-and-csharp-generics.md)
- [泛型和反射](generics-and-reflection.md)
- [运行时中的泛型](generics-in-the-run-time.md)

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/types.md#constructed-types)。

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Generic>
- [C# 编程指南](../index.md)
- [类型](../types/index.md)
- [\<typeparam>](../xmldoc/typeparam.md)
- [\<typeparamref>](../xmldoc/typeparamref.md)
- [.NET 中的泛型](../../../standard/generics/index.md)
