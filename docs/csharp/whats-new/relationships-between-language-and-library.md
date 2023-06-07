---
title: 语言功能与库类型之间的关系 |Microsoft 文档
description: 语言功能通常依赖于要实现的库类型。 了解该关系。
ms.date: 07/20/2017
ms.openlocfilehash: abf15385da3756c35db2df822cc2e11e9edf5758
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174096"
---
# <a name="relationships-between-language-features-and-library-types"></a>语言功能与库类型之间的关系

C# 语言定义要求标准库拥有某些类型以及这些类型的特定可访问成员。 编译器针对多种不同语言功能生成使用这些必需类型和成员的代码。 如有必要，在针对尚未部署这些类型或成员的环境编写代码时，可使用包含较新版本的语言所需类型的 NuGet 包。

此标准库功能的依赖项自其第一个版本起就是 C# 语言的一部分。 在该版本中，相关示例包括：

* <xref:System.Exception> - 用于编译器生成的所有异常。
* <xref:System.String> - C# `string` 类型是 <xref:System.String> 的同义词。
* <xref:System.Int32> - `int` 的同义词。

第一个版本很简单：编译器和标准库一起提供，且各自都只有一个版本。

后续版本的 C# 偶尔会向依赖项添加新类型或成员。 相关示例包括：<xref:System.Runtime.CompilerServices.INotifyCompletion>、<xref:System.Runtime.CompilerServices.CallerFilePathAttribute> 和 <xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>。 C# 7.0 继续添加 <xref:System.ValueTuple> 的依赖项，以实现[元组](../language-reference/builtin-types/value-tuples.md)语言功能。

语言设计团队致力于最小化符合标准的标准库所需的类型和成员的外围应用。 该目标针对新库功能无缝集成到语言的简洁设计进行了平衡。 未来版本的 C# 中还会包括需要标准库中的新类型和成员的新功能。 必须了解如何管理工作中的这些依赖项。

## <a name="managing-your-dependencies"></a>管理依赖项

C# 编译器工具现在从支持的平台上 .NET 库的发布周期分离。 实际上，不同的 .NET 库有不同的发布周期：Windows 上的 .NET Framework 作为 Windows 更新发布，.NET Core 在单独的计划中提供，Xamarin 版本的库更新随适用于每个目标平台的 Xamarin 工具提供。

大多数时候，用户都不会注意到这些更改。 但是，如果使用的较新版本语言需要该平台上的 .NET 库中尚未包含的功能，则会引用 NuGet 包以提供这些新类型。
应用支持的平台会随着新框架的安装而更新，因此可以删除额外的引用。

此分离意味着即使面向没有相应框架的计算机，仍可使用新语言功能。
