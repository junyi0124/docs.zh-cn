---
title: .NET 类库
description: 了解如何通过 .NET 类库，将实用功能组合为可供多个应用程序使用的模块。
author: richlander
ms.date: 06/20/2016
ms.assetid: a67484c3-fe92-44d8-8fa3-36fa2071d880
ms.openlocfilehash: bbf8d5fa44af931e39f2bf1344c13f9d9d9bfe05
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823948"
---
# <a name="net-class-libraries"></a>.NET 类库

类库是 .NET 的[共享库](https://en.wikipedia.org/wiki/Library_%28computing%29#Shared_libraries)概念。 通过类库可将实用功能组件化为可供多个应用程序使用的模块。 还可使用类库加载应用程序启动时不需要或未知的功能。 类库通过 [.NET 程序集文件格式](assembly/file-format.md)进行描述。

有三种类型的类库可供使用：

* **平台特定** 的类库可访问给定平台（例如，.NET Framework、Xamarin、iOS）中的所有 API，但只有面向该平台的应用和库可使用该类库。
* **可移植** 类库可访问 API 的子集，并且可供面向多个平台的应用和库使用。
* .NET Standard 类库将平台专用库概念和可移植库概念合并到一个模型中，以同时获取两方面的优势。

## <a name="platform-specific-class-libraries"></a>平台特定的类库

平台特定的类库绑定到单个 .NET 实现（例如，Windows 上的 .NET Framework），因此它可以在已知的执行环境上接收重要的依赖项。 此类环境会公开一组已知 API（.NET 和 OS API），维护并公开预期状态（例如，Windows 注册表）。

创建平台特定的库的开发人员可充分利用基础平台。 该库只会在给定平台上运行，因此不需要平台检查和其他形式的条件代码（针对多个平台取模单个源代码）。

平台特定的库一直是 .NET Framework 的主要类库类型。 即使出现了其他 .NET 实现，平台特定的库也仍然是最主要的类库类型。

## <a name="portable-class-libraries"></a>可移植类库

可移植库支持多个 .NET 实现。 该库仍可在已知执行环境上接收依赖项，不过该环境是一种合成环境，由一组具体 .NET 实现的交集生成。 公开的 API 和平台假设是平台特定的库可用的子集。

创建可移植库时，需选择平台配置。 平台配置是需要支持的平台集（例如，.NET Framework 4.5+、Windows Phone 8.0+）。 要支持的平台越多，可生成的 API 和平台假设就越少，公分母越小。 这一特性可能最初会令人感到疑惑，因为人们常认为“越多越好”，但却发现更多的支持平台带来的可用 API 更少。

许多库开发人员已经从由一个源开发多个平台特定的库（使用条件编译指令）转向开发可移植库。 有[多种方法](https://blog.stephencleary.com/2012/11/portable-class-library-enlightenment.html)可在可移植库中访问平台特定的功能，其中“诱饵替换”是目前最广为接受的方法。

## <a name="net-standard-class-libraries"></a>.NET Standard 类库

.NET Standard 库是平台特定的库和可移植库概念的替代。 .NET Core 库可从基础平台公开所有功能（无合成平台或平台交集），就此而言，它是平台特定的库。 该库可在所有支持平台上运行，就此而言，它是可移植库。

.NET Standard 公开一组库协定。 .NET 实现必须完全支持每个协定，否则就全都不支持。 因此，每个实现都支持一组 .NET Standard 协定。 得出的必然结果是，.NET Standard 类库在支持其协定依赖项的平台上受到支持。

.NET Standard 不公开整个 .NET Framework 的功能（也不将此作为目标），但相比可移植类库，其公开的 API 更多。 随着时间推移，将添加更多的 API。

下列平台支持 .NET Standard 库：

* .NET Core
* .NET Framework
* Mono
* Xamarin.iOS、Xamarin.Mac、Xamarin.Android
* 通用 Windows 平台 (UWP)
* Windows
* Windows Phone
* Windows Phone Silverlight

有关详细信息，请参阅 [.NET Standard](net-standard.md)。

## <a name="mono-class-libraries"></a>Mono 类库

Mono 支持多种类库，包括上述三种类型的库。 Mono 常被（正确地）视为 .NET Framework 的跨平台实现。 部分原因是，平台特定的 .NET Framework 库可在 Mono 运行时上运行，而无需修改或重新编译。 创建可移植库之前，此特性就已存在，因此在 .NET Framework 和 Mono 之间启用二进制可移植性是显而易见的选择（虽然它只能单向运行）。
