---
title: 类型提供程序
description: 了解 F# 类型提供程序如何成为提供在程序中使用的类型、属性和方法的组件。
ms.date: 04/02/2018
ms.openlocfilehash: eae64d2e318ee93f0b8d5b91f0c6da6c91743527
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202109"
---
# <a name="type-providers"></a>类型提供程序

F# 类型提供程序是提供在程序中使用的类型、属性和方法的组件。 类型提供程序会生成被称为“提供的类型”内容，它们由 F# 编译器生成并基于外部数据源。

例如，SQL 的 F# 类型提供程序可生成在关系数据库中表示表格和列的类型。 事实上，这是 [SQLProvider](https://fsprojects.github.io/SQLProvider/) 类型提供程序实现的操作。

提供的类型依赖于类型提供程序的输入参数。 此类输入可以是示例数据源（例如 JSON 架构文件）、直接指向外部服务的 URL，也可以是数据源的连接字符串。 类型提供程序还可确保仅按需展开类型组；也就是说，如果类型实际上是由你的程序引用的，则将其展开。 这允许以强类型的方式直接按需集成大型信息空间（如联机数据市场）。

## <a name="generative-and-erased-type-providers"></a>生成的和已擦除的类型提供程序

类型提供程序采用两种形式：“生成”和“擦除”。

生成类型提供程序会生成可作为 .NET 类型写入到生成它们的程序集中的类型。 这样，就可从其他程序集的代码中使用它们。 这意味着数据源的类型化表示形式通常必须是可与 .NET 类型一起表示的形式。

擦除类型提供程序会生成只能在生成它们的程序集或项目中使用的类型。 这些类型是暂时的；也就是说，它们不会写入程序集，也不能由其他程序集中的代码使用。 它们可包含延迟成员，让你能够使用从可能无限的信息空间提供的类型。 对于使用一小部分大型互连数据源来说，它们非常有用。

## <a name="commonly-used-type-providers"></a>常用类型提供程序

下列广泛使用的库包含适合不同用途的类型提供程序：

- [FSharp.Data](https://fsharp.github.io/FSharp.Data/) 包含用于 JSON、XML、CSV 和 HTML 文档格式和资源的类型提供程序。
- [SQLProvider](https://fsprojects.github.io/SQLProvider/) 通过对象映射以及 F# 针对这些数据源的 LINQ 查询，提供对关系数据库的强类型访问。
- [FSharp.Data.SqlClient](https://fsprojects.github.io/FSharp.Data.SqlClient/) 包含一组类型提供程序，用于在 F# 中对 T-SQL 进行编译时检查。
- [Azure 存储类型提供程序](https://fsprojects.github.io/AzureStorageTypeProvider/) 提供 Azure Blob、表和队列的类型，让你无需在整个程序中将资源名称指定为字符串就能访问这些资源。
- [FSharp.Data.GraphQL](https://fsprojects.github.io/FSharp.Data.GraphQL/index.html) 包含 GraphQLProvider，它基于 URL 指定的 GraphQL 服务器提供类型。

根据需要，你可以[创建自己的自定义类型提供程序](creating-a-type-provider.md)，也可以引用其他人创建的类型提供程序。 例如，假定你的组织具有一种数据服务，该服务提供了数量不断增长的命名数据集，每个数据集都有其自己的稳定数据架构。 你可选择创建一种类型提供程序，该程序不仅读取架构，还以强类型方式将最新的可用数据集提供给程序员。

## <a name="see-also"></a>请参阅

- [教程：创建类型提供程序](creating-a-type-provider.md)
- [F# 语言参考](../../language-reference/index.md)
