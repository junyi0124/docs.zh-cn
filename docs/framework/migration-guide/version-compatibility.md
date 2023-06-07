---
title: .NET Framework 的版本兼容性
description: 了解 .NET Framework 版本之间的兼容性，包括后向兼容性和并行执行。
ms.date: 04/02/2019
helpviewer_keywords:
- .NET Framework, version compatibility
- .NET Framework, compatibility with earlier versions
- .NET Framework versions, compatibility
ms.assetid: 2f25e522-456a-48c3-8a53-e5f39275649f
ms.openlocfilehash: 2be9c4e12d6a613e7f1062ec7492b0b99203f39d
ms.sourcegitcommit: 48466b8fb7332ececff5dc388f19f6b3ff503dd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93400691"
---
# <a name="version-compatibility"></a>版本兼容性

向后兼容性表示为某个平台的特定版本开发的应用程序将在该平台的更高版本上运行。 .NET Framework 尝试最大程度地支持后向兼容性：为某个版本的 .NET Framework 编写的源代码应在更高版本的 .NET Framework 上编译，而在某个版本的 .NET Framework 上运行的二进制文件的行为方式应与其在更高版本的 .NET Framework 上的行为方式相同。

## <a name="version-compatibility-for-apps"></a><a name="Apps"></a> 应用的版本兼容性

默认情况下，应用将在其目标 .NET Framework 版本上运行。 如果该版本不存在且应用配置文件未定义支持的版本，则可能出现 .NET Framework 初始化错误。 在此情况下，尝试运行应用程序将失败。

若要定义运行应用的特定版本，请将一个或多个 [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 元素添加到应用的配置文件中。 每个 `<supportedRuntime>` 元素都列出了支持的运行时版本，第一个元素指定了优先级最高的版本，最后一个元素指定了优先级最低的版本。

```xml
<configuration>
   <startup>
      <supportedRuntime version="v2.0.50727" />
      <supportedRuntime version="v4.0" />
   </startup>
</configuration>
```

有关详细信息，请参阅[如何：将应用配置为支持 .NET Framework 4 或 4.x](how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)。

## <a name="version-compatibility-for-components"></a>组件的版本兼容性

应用可以控制运行它的 .NET Framework 的版本，但组件不能。 因为组件和类库在特定应用的上下文中加载，所以它们会自动在运行应用的 .NET Framework 版本上运行。

由于存在此限制，因此兼容性保证对组件特别重要。 从 .NET Framework 4 开始，你可以通过将 <xref:System.Runtime.Versioning.ComponentGuaranteesAttribute?displayProperty=nameWithType> 特性应用于某个组件，来指定希望该组件与多个版本的兼容程度。 工具可使用此特性来检测组件的将来版本中的兼容性保证的潜在冲突。

## <a name="backward-compatibility"></a>向后兼容性

.NET Framework 4.5 及更高版本与使用早期版本的 .NET Framework 生成的应用向后兼容。 换句话说，使用早期版本的 .NET Framework 生成的应用和组件将运行，而无需在 .NET Framework 4.5 及更高版本上进行修改。 但在默认情况下，应用在其进行开发的公共语言运行时版本上运行，因此必须提供配置文件，使应用能在 .NET Framework 4.5 及更高版本上运行。 有关详细信息，请参阅本文前面的[应用的版本兼容性](#Apps)一节。

实际上，.NET Framework 中看似无关紧要的更改和编程技术上的更改会损坏此兼容性。 例如，.NET Framework 4.5 中的性能改进会公开早期版本中未出现的争用条件。 同样，使用 .NET Framework 程序集的硬编码路径，执行与特定版本的 .NET Framework 的相等比较，以及使用反射获取私有字段的值都不是向后兼容的做法。 此外，每个版本的 .NET Framework 都包含 Bug 修复和可能影响某些应用程序和组件的兼容性的安全相关更改。

如果应用或组件在 .NET Framework 4.5（包括其单点版本，即 .NET Framework 4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2 或 4.8）上未按预期运行，请使用以下清单：

- 如果你开发的应用是为在 .NET Framework 4.0 以后的任何 .NET Framework 版本上运行，请参阅[应用程序兼容性](application-compatibility.md)，以生成目标 .NET Framework 版本与运行应用的版本之间的更改列表。

- 如果使用 .NET Framework 3.5 应用，另请参阅 [.NET Framework 4 迁移问题](net-framework-4-migration-issues.md)。

- 如果使用 .NET Framework 2.0 应用，另请参阅 [.NET Framework 3.5 SP1 中的更改](/previous-versions/dotnet/articles/dd310284(v=msdn.10))。

- 如果使用 .NET Framework 1.1 应用，另请参阅 [.NET Framework 2.0 中的更改](/previous-versions/aa570326(v=msdn.10))。

- 如果要重新编译现有源代码以在 .NET Framework 4.5 或其单点版本上运行，或如果要通过现有源代码库开发面向 .NET Framework 4.5 或其单点版本的新版本应用或组件，请查看[类库中的过时内容](../whats-new/whats-obsolete.md)以了解过时的类型和成员，并应用所述的解决方法。 （以前编译的代码将继续针对已标记为过时的类型和成员运行。）

- 如果确定 .NET Framework 4.5 中的更改已损坏应用，请查看[运行时设置架构](../configure-apps/file-schema/runtime/index.md)（特别是 [\<AppContextSwitchOverrides> 元素](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)），以确定是否能够在应用的配置文件中使用运行时设置来还原以前的行为。

- 如果遇到未记录的问题，请在[适用于 .NET 的开发人员社区网站](https://aka.ms/feedback/report?space=61)中新开一个问题，或在 [Microsoft/dotnet GitHub 存储库](https://github.com/microsoft/dotnet/issues)中新开一个问题。

## <a name="side-by-side-execution"></a>并行执行

如果找不到解决问题的适当方法，请记住，.NET Framework 4.5（或其单点版本之一）与版本 1.1、2.0 和 3.5 并行运行，并且是取代版本 4 的就地更新。 对于以版本 1.1、2.0 和 3.5 为目标的应用程序，你可以在目标计算机上安装适当的 .NET Framework 版本以在其最佳环境中运行该应用程序。 有关并行执行的详细信息，请参阅[并行执行](../deployment/side-by-side-execution.md)。

## <a name="see-also"></a>请参阅

- [新增功能](../whats-new/index.md)
- [类库中过时的内容](../whats-new/whats-obsolete.md)
- [应用程序兼容性](application-compatibility.md)
- [.NET Framework 官方支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)
- [.NET Framework 4 迁移问题](net-framework-4-migration-issues.md)
