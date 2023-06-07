---
title: 运行时和重定向更改 - .NET Framework
description: 了解 .NET Framework 中的应用程序兼容性，以及迁移到其他版本时运行时和重定目标更改对应用程序兼容性的影响。
ms.date: 10/29/2019
helpviewer_keywords:
- application compatibility
- .NET Framework application compatibility
- .NET Framework changes
ms.assetid: c4ba3ff2-fe59-4c5d-9e0b-86bba3cd865c
ms.openlocfilehash: 26f36dd34c6c5ecae8fc5ab373ff60d9e56f8245
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475484"
---
# <a name="application-compatibility-in-the-net-framework"></a>.NET Framework 中的应用程序兼容性

兼容性是每个 .NET 版本的重要目标。 兼容性可确保每个版本都具有累加特征，以便旧版本继续正常使用。 然而，更改旧功能（例如，用来提升性能、解决安全性问题或修复 bug 等）可能会导致在更高版本 .NET 上运行的现有代码或应用程序出现兼容性问题。

每个应用都通过以下方式面向特定版本的 .NET Framework：

- 在 Visual Studio 中定义目标框架。
- 在项目文件中指定目标框架。
- 向源代码应用 <xref:System.Runtime.Versioning.TargetFrameworkAttribute>。

从一个 .NET Framework 版本迁移到另一个版本时，需注意以下两种类型的更改：

- [运行时更改](#runtime-changes)
- [重定目标更改](#retargeting-changes)

## <a name="runtime-changes"></a>运行时更改

运行时问题是指当新的运行时出现在计算机上并且应用的行为更改时出现的问题。 如果应用在比目标 .NET 版本更高的版本上运行，.NET Framework 会通过怪异的行为来模拟旧版目标版本。 应用在较高的版本中运行，但其行为方式如同运行于较低版本中一样。 .NET Framework 各版本之间的许多兼容性问题都是通过这种怪异的模型进行缓解。 例如，如果编译了用于 .NET Framework 4.0 的二进制文件，但在安装了 .NET Framework 4.5 或更高版本的计算机上运行它，它就会采用 .NET Framework 4.0 兼容模式运行。 这意味着，较高版本中的许多更改不影响此二进制文件。

应用程序面向的 .NET Framework 版本取决于运行代码的应用程序域的输入程序集的目标版本。 该应用程序域中加载的所有附加程序集都面向此版本。 例如，如果是可执行文件，则该可执行文件面向的版本就是一个兼容模式，应用程序域中的所有程序集都在这个兼容模式下运行。

如果要查看适用于你的环境的运行时更改列表，请选择当前面向的 .NET Framework 版本，然后选择要迁移到的版本：

[!INCLUDE[versionselector](../../../includes/migration-guide/runtime/versionselector.md)]

## <a name="retargeting-changes"></a>重定目标更改

重定向更改是指在将程序集重写编译为面向较高版本时发生的更改。 面向较高版本意味着程序集会选择加入新功能，同时可能存在与旧功能的兼容性问题。

如果要查看适用于你的环境的重定目标更改列表，请选择当前面向的 .NET Framework 版本，然后选择要迁移到的版本：

[!INCLUDE[versionselector](../../../includes/migration-guide/retargeting/versionselector.md)]

## <a name="impact-classification"></a>影响分类

在介绍运行时和重定向更改的主题中（例如，[从 4.7.2 迁移到 4.8 的重定向更改](retargeting/4.7.2-4.8.md)），已根据其预期影响为各项进行了分类，如下所示：

**Major**\
显著的更改，可影响大量应用或需要修改大量代码。

**Minor**\
影响少量应用或需要修改少量代码的更改。

**边缘情况**\
仅在少数非常特定的情况下影响应用的更改。

**透明**\
对应用的开发人员或用户没有明显影响的更改。 不需要由于此更改而修改应用。

## <a name="see-also"></a>请参阅

- [版本和依赖关系](versions-and-dependencies.md)
- [新增功能](../whats-new/index.md)
- [过时内容](../whats-new/whats-obsolete.md)
