---
title: .NET Framework 中的过时功能
description: 了解 .NET 类库如何将成员标记为已过时。 了解 ObsoleteAttribute 属性，并了解如何处理过时的类型和成员等。
ms.date: 04/02/2019
helpviewer_keywords:
- obsolete [.NET Framework]
- what's obsolete [.NET Framework]
- deprecated [.NET Framework]
ms.assetid: d356a43a-73df-4ae2-a457-b9628074c7cd
ms.openlocfilehash: 93c2015c873b9de7b6c1b94224fa4033863aecfd
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558493"
---
# <a name="whats-obsolete-in-the-net-framework-class-library"></a>.NET Framework 类库中过时的内容

.NET 随时间变化。 每个新版本都添加了提供新功能的新类型和类型成员。 现有类型和成员也会随着时间推移而变化。 例如，某些类型变得不太重要，因为它们支持的技术由新技术所替代，而某些方法由在某种程度上更高级的新方法所取代。

.NET Framework 和公共语言运行时会努力支持向后兼容（允许使用一个版本的 .NET Framework 开发的应用程序在下一版本的 .NET Framework 上运行）。 这便难以仅仅删除类型或类型成员。 相反，.NET 通过将类型或类型成员标记为已过时或已弃用，来指示应不再使用它。 弃用某个类型或成员涉及对它进行标记，以便开发人员知道它将消失，从而有时间来响应其删除。 但是，使用该类型或成员的现有代码会继续在新版本的 .NET 中运行。

> [!NOTE]
> 术语“已过时”和“已弃用”在应用于 .NET 类型和成员时含义相同。

## <a name="the-obsoleteattribute-attribute"></a>ObsoleteAttribute 特性

.NET Framework 通过使用 <xref:System.ObsoleteAttribute> 特性标记类型或类型成员来指示它已过时。 将该特性应用于某个类型或成员指示该类型或成员会在将来某个版本的 .NET Framework 中删除，但不会破坏使用该成员的已编译代码。

除了指示类型或类型成员已过时之外，<xref:System.ObsoleteAttribute> 还定义编译器如何处理包含该类型或成员的源代码。 编译器可以编译代码但发出警告消息，也可以将该类型或成员的使用视为错误。 在第一种情况下，代码可以成功编译，但警告消息会指示类型或成员已过时。 在第二种情况下，编译会失败。

即使编译生成错误而不是警告消息，<xref:System.ObsoleteAttribute> 也不会影响运行时行为。 也就是说，使用该类型或成员并且已成功编译的应用程序会始终成功运行。 只有重新编译使用该类型或成员的应用程序的尝试会失败。

## <a name="how-to-handle-obsolete-types-and-members"></a>如何处理已过时的类型和成员

升级和重新编译现有代码时，使用会在应用程序中生成编译器警告的已过时类型或成员是完全可以接受的。 但是，应查看编译器警告消息，以确定是否应更改应用程序代码。 如果该消息不指向合适的替代方法，则应执行以下任一操作：

- 通过删除该类型或成员的使用（如果可能）来更改代码。

     \- 或 -

- 查看有关此技术领域的文档，以确定如何响应弃用情况。

可以选择不针对更高版本的 .NET Framework 重新编译现有代码。 相反，可以指定针对其运行现有已编译代码的 .NET Framework 版本。 例如，假设你有一个名为 app1.exe 并且针对 .NET Framework 3.5 进行了编译的应用程序，但是希望针对 .NET Framework 4.5 运行该应用程序。 这需要执行以下步骤：

1. 为主可执行文件创建一个配置文件并将它命名为 *appName*.exe.config，其中 *appName* 是应用程序可执行文件的名称。 对于我们示例中名为 app1.exe 的应用程序，会创建一个名为 app1.exe.config 的配置文件。

2. 向该配置文件添加以下内容：

    ```xml
    <configuration>
       <startup>
          <supportedRuntime version="v4.0" />
       </startup>
    </configuration>
    ```

为针对特定版本的 .NET Framework，请将下列其中一个字符串值分配给 `version` 特性：

|.NET Framework 版本|`version` 字符串|
|-|-|
|4.8|v4.0|
|4.7（包括 4.7.1 和 4.7.2）|v4.0|
|4.6（包括 4.6.1 和 4.6.2）|v4.0|
|4.5（包括 4.5.1 和 4.5.2）|v4.0|
|4|v4.0|
|3.5|v2.0.50727|
|2.0|v2.0.50727|
|1.1|v1.1.4322|
|1.0|v1.0.3705|

## <a name="obsolete-apis-for-net-framework-45-and-later-versions"></a>.NET Framework 4.5 及更高版本的过时 API

- [过时类型](obsolete-types.md)
- [过时成员](obsolete-members.md)

## <a name="obsolete-apis-for-previous-versions"></a>以前版本的过时 API

- [.NET Framework 4 中的已过时类型](/previous-versions/dotnet/netframework-4.0/ee461503(v=vs.100))
- [.NET Framework 4 中的已过时成员](/previous-versions/dotnet/netframework-4.0/ee471421(v=vs.100))
- [.NET Framework 3.5 过时列表](/previous-versions/cc835481(v=msdn.10))
- [.NET Framework 2.0 过时列表](/previous-versions/aa497286(v=msdn.10))

## <a name="see-also"></a>请参阅

- [\<supportedRuntime> 元素](../configure-apps/file-schema/startup/supportedruntime-element.md)
