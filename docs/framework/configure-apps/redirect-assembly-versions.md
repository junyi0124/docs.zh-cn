---
title: 重定向程序集版本
description: 将编译时绑定引用重定向到不同版本的 .NET 程序集、第三方程序集或你自己的应用程序集。
ms.date: 03/30/2017
helpviewer_keywords:
- assembly binding, redirection
- redirecting assembly binding to earlier version
- configuration [.NET Framework], applications
- application configuration [.NET Framework]
- assemblies [.NET Framework], binding redirection
ms.assetid: 88fb1a17-6ac9-4b57-8028-193aec1f727c
ms.openlocfilehash: f0db5c32ba12b8e5313ca363e82260d66a7c010f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166893"
---
# <a name="redirecting-assembly-versions"></a>重定向程序集版本

你可以将编译时绑定引用重定向到 .NET Framework 程序集、第三方程序集或你自己的应用的程序集。 你还可以重定向应用，以通过多种方式使用不同版本的程序集：通过发布服务器策略、通过应用配置文件或通过计算机配置文件。 本文讨论了程序集绑定在 .NET Framework 中的工作原理以及如何对其进行配置。

<a name="BKMK_Assemblyunificationanddefaultbinding"></a>

## <a name="assembly-unification-and-default-binding"></a>程序集统一和默认绑定

 到 .NET Framework 程序集的绑定有时会通过称为 *“程序集统一”* 的过程进行重定向。 .NET Framework 包括一个公共语言运行时版本和构成类型库的约二十个 .NET Framework 程序集。 运行时将这些 .NET Framework 程序集视为单个单元。 默认情况下，当启动应用时，由运行时运行的所有对代码中的类型的引用都将定向到具有与进程中加载的运行时相同的版本号的 .NET Framework 程序集。 此模型发生的重定向是运行时的默认行为。

 例如，如果你的应用程序引用 System.XML 命名空间中的类型，并且是使用 .NET Framework 4.5 生成的，则该应用程序包含对运行时版本4.5 随附的 System.XML 程序集的静态引用。 如果想要重定向绑定引用，以指向 System.XML 程序集（随附 .NET Framework 4），你可以将重定向信息放在应用配置文件中。 统一的 .NET Framework 程序集的配置文件中的绑定重定向将取消该程序集的统一。

 此外，如果有多个可用版本，可能需要手动重定向第三方程序集的程序集绑定。

<a name="BKMK_Redirectingassemblyversionsbyusingpublisherpolicy"></a>

## <a name="redirecting-assembly-versions-by-using-publisher-policy"></a>通过使用发布者策略重定向程序集版本

 程序集的供应商可以通过包括发布服务器策略文件与新的程序集，将应用定向到较新版本的程序集。 位于全局程序集缓存中的发布服务器策略文件包含程序集重定向设置。

 每个 *主要*、*次要* 版本的程序集都具有其自己的发布服务器策略文件。 例如，从版本 2.0.2.222 到 2.0.3.000 和从版本 2.0.2.321 到版本 2.0.3.000 的重定向都转到同一文件中，因为它们与版本 2.0 相关联。 但是，从版本 3.0.0.999 到版本 4.0.0.000 的重定向则转入版本 3.0.999 的文件。 每个主要版本的 .NET Framework 都具有其自己的发布服务器策略文件。

 如果存在某一程序集的发布服务器策略文件，则在检查过该程序集的清单和应用配置文件后，运行时将检查该文件。 仅当新的程序集与被重定向的程序集向后兼容时，供应商才会使用发布服务器策略文件。

 你通过在应用配置文件指定设置，跳过应用的发布服务器策略，如 [跳过发布者策略部分](#bypass_PP)中所述。

<a name="BKMK_Redirectingassemblyversionsattheapplevel"></a>

## <a name="redirecting-assembly-versions-at-the-app-level"></a>在应用级别重定向程序集版本

 通过应用配置文件，有几种不同的技术来更改你的应用绑定行为：你可以手动编辑该文件、可以依赖于自动绑定重定向或可以通过跳过发布服务器策略指定绑定行为。

### <a name="manually-editing-the-app-config-file"></a>手动编辑应用配置文件

 你可以手动编辑应用配置文件，解决程序集问题。 例如，供应商可能会发布你的应用使用的较新版本的程序集，而没有提供发布服务器策略，因为他们不保证向后兼容性，你可以通过将程序集绑定信息放置在如下所示的应用配置文件中，定向你的应用，以使用较新的程序集版本。

```xml
<dependentAssembly>
  <assemblyIdentity name="someAssembly"
    publicKeyToken="32ab4ba45e0a69a1"
    culture="en-us" />
  <bindingRedirect oldVersion="7.0.0.0" newVersion="8.0.0.0" />
</dependentAssembly>
```

### <a name="relying-on-automatic-binding-redirection"></a>依赖于自动绑定重定向

在 Visual Studio 中创建面向 .NET Framework 4.5.1 或更高版本的桌面应用程序时，应用程序将使用自动绑定重定向。 这意味着如果两个组件引用同一强名称程序集的不同版本，则运行时会自动添加绑定重定向到输出应用配置 (app.config) 文件中的程序集的较新版本。 此重定向将重写可能会发生的程序集统一。 不修改源 app.config 文件。 例如，假设你的应用直接引用带外 .NET Framework 组件，但使用面向同一组件的较旧版本的第三方库。 在编译该应用时，将修改输出应用配置文件以包含绑定重定向到较新版本的组件。 如果创建一个 Web 应用，你将收到有关绑定冲突的生成警告，这反过来将为你提供将必要的绑定重定向添加到源 Web 配置文件的选项。

如果你在编译时手动添加绑定重定向到源 app.config 文件，则 Visual Studio 会尝试基于你添加的绑定重定向来统一程序集。 例如，假设你对某一程序集插入以下绑定重定向：

`<bindingRedirect oldVersion="3.0.0.0" newVersion="2.0.0.0" />`

如果你的应用中的另一个项目引用了同一程序集的版本 1.0.0.0，则自动绑定重定向将添加以下条目到输出 app.config 文件，以便该应用在此程序集的版本 2.0.0.0 上统一：

`<bindingRedirect oldVersion="1.0.0.0" newVersion="2.0.0.0" />`

如果你的应用面向 .NET Framework 的旧版本，则可以启用自动绑定重定向。 您可以通过在任何程序集的 app.config 文件中提供绑定重定向信息，或关闭绑定重定向功能，来重写此默认行为。 有关如何打开或关闭此功能的信息，请参阅 [如何：启用和禁用自动绑定重定向](how-to-enable-and-disable-automatic-binding-redirection.md)。

<a name="bypass_PP"></a>

### <a name="bypassing-publisher-policy"></a>跳过发布服务器策略

 如有必要，你可以在应用配置文件中重写发布服务器策略。 例如，声称向后兼容的程序集的新版本也会中断应用。 如果要跳过发布服务器策略，请将 [\<publisherPolicy>](./file-schema/runtime/publisherpolicy-element.md) 元素添加到 [\<dependentAssembly>](./file-schema/runtime/dependentassembly-element.md) 应用程序配置文件中的元素，并将 " **应用** 属性" 设置为 " **否**"，这将替代以前的 **"是"** 设置。

 `<publisherPolicy apply="no" />`

 跳过发布服务器策略来保持应用为你的用户运行，但要确保将问题报告给程序集供应商。 如果程序集具有发布服务器策略文件，则供应商应确保该程序集向后兼容并且该客户端可以尽可能多的使用新版本。

<a name="BKMK_Redirectingassemblyversionsatthemachinelevel"></a>

## <a name="redirecting-assembly-versions-at-the-machine-level"></a>在计算机级别重定向程序集版本

 可能存在极少数情况，当计算机管理员想要计算机上所有的应用都使用程序集的某一特定版本时。 例如，管理员可能希望每个应用都使用特定的程序集版本，因为该版本可修复安全漏洞。 如果某个程序集在计算机配置文件中进行重定向，则该计算机上的所有使用旧版本的应用都将被定向为使用新版本。 计算机配置文件将重写应用配置文件和发布服务器策略文件。 此文件位于 %*runtime install path*%\Config 目录中。 通常，.NET Framework 安装在 %drive%\Windows\Microsoft.NET\Framework 目录中。

<a name="BKMK_Specifyingassemblybindinginconfigurationfiles"></a>

## <a name="specifying-assembly-binding-in-configuration-files"></a>在配置文件中指定程序集绑定

 使用相同的 XML 格式指定绑定重定向，无论它位于应用配置文件、计算机配置文件还是位于发布服务器策略文件中。 若要将一个程序集版本重定向到另一个，请使用 [\<bindingRedirect>](./file-schema/runtime/bindingredirect-element.md) 元素。 **oldVersion** 特性可以指定单个程序集版本或一系列版本。 `newVersion` 特性将指定单个版本。  例如， `<bindingRedirect oldVersion="1.1.0.0-1.2.0.0" newVersion="2.0.0.0"/>` 指定运行时应使用版本 2.0.0.0 而不是 1.1.0.0 和 1.2.0.0 之间的程序集版本。

 以下代码示例演示了各种绑定重定向方案。 该示例对一系列 `myAssembly`的版本指定了一个重定向，并对 `mySecondAssembly`指定了一个单一绑定重定向。 该示例还指定发布服务器策略文件不会代替 `myThirdAssembly`的绑定重定向。

 若要绑定程序集，必须在标记中使用 **xmlns** 特性指定字符串 "urn：架构-microsoft com： .asm. v1" [\<assemblyBinding>](./file-schema/runtime/assemblybinding-element-for-runtime.md) 。

```xml
<configuration>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="myAssembly"
          publicKeyToken="32ab4ba45e0a69a1"
          culture="en-us" />
        <!-- Assembly versions can be redirected in app,
          publisher policy, or machine configuration files. -->
        <bindingRedirect oldVersion="1.0.0.0-2.0.0.0" newVersion="3.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
        <assemblyIdentity name="mySecondAssembly"
          publicKeyToken="32ab4ba45e0a69a1"
          culture="en-us" />
             <bindingRedirect oldVersion="1.0.0.0" newVersion="2.0.0.0" />
      </dependentAssembly>
      <dependentAssembly>
      <assemblyIdentity name="myThirdAssembly"
        publicKeyToken="32ab4ba45e0a69a1"
        culture="en-us" />
        <!-- Publisher policy can be set only in the app
          configuration file. -->
        <publisherPolicy apply="no" />
      </dependentAssembly>
    </assemblyBinding>
  </runtime>
</configuration>
```

### <a name="limiting-assembly--bindings-to-a-specific-version"></a>限制到特定版本的程序集绑定

 您可以使用**appliesTo** [\<assemblyBinding>](./file-schema/runtime/assemblybinding-element-for-runtime.md) 应用程序配置文件中的元素上的 appliesTo 特性，将程序集绑定引用重定向到 .NET Framework 的特定版本。 此可选特性用 .NET Framework 版本号来指示其适用的版本。 如果没有指定 appliesTo 特性，\<assemblyBinding> 元素将适用于 .NET Framework 的所有版本 。

 例如，若要重定向 .NET Framework 3.5 程序集的程序集绑定，应在你的应用配置文件中包括以下 XML 代码。

```xml
<runtime>
  <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1"
    appliesTo="v3.5">
    <dependentAssembly>
      <!-- assembly information goes here -->
    </dependentAssembly>
  </assemblyBinding>
</runtime>
```

 应按版本顺序输入重定向信息。 例如，先输入 .NET Framework 3.5 程序集的程序集绑定重定向信息，再输入 .NET Framework 4.5 程序集的绑定重定向信息。 最后，输入任何因不使用 **appliesTo** 特性而适用于所有版本的 .NET Framework 的.NET Framework 程序集重定向的程序集绑定重定向信息。 如果发生重定向冲突，请使用配置文件中的第一个匹配的重定向语句。

 例如，若要将一个引用重定向到 .NET Framework 3.5 程序集，而将另一个引用重定向到 .NET Framework 4 程序集，则使用以下伪代码中所示的模式。

```xml
<assemblyBinding xmlns="..." appliesTo="v3.5 ">
  <!--.NET Framework version 3.5 redirects here -->
</assemblyBinding>

<assemblyBinding xmlns="..." appliesTo="v4.0.30319">
  <!--.NET Framework version 4.0 redirects here -->
</assemblyBinding>

<assemblyBinding xmlns="...">
  <!-- redirects meant for all versions of the runtime -->
</assemblyBinding>
```

## <a name="see-also"></a>请参阅

- [如何：启用和禁用自动绑定重定向](how-to-enable-and-disable-automatic-binding-redirection.md)
- [\<bindingRedirect> 元素](./file-schema/runtime/bindingredirect-element.md)
- [程序集绑定重定向安全权限](assembly-binding-redirection-security-permission.md)
- [.NET 中的程序集](../../standard/assembly/index.md)
- [使用程序集编程](../../standard/assembly/index.md)
- [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)
- [配置应用程序](index.md)
- [运行时设置架构](./file-schema/runtime/index.md)
- [配置文件架构](./file-schema/index.md)
- [如何：创建发行者策略](how-to-create-a-publisher-policy.md)
