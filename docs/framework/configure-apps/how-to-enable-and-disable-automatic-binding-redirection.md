---
title: 启用或禁用自动生成绑定重定向
description: 阅读如何启用或禁用自动绑定重定向。 此功能影响 .NET Framework 4.5.1 或更高版本的桌面应用和 web 应用。
ms.date: 10/30/2018
helpviewer_keywords:
- side-by-side execution, assembly binding redirection
- assemblies [.NET Framework], binding redirection
ms.assetid: 5fca42f3-bdce-4b81-a704-61e42c89d3ba
ms.openlocfilehash: b099ab4958b1cf41b76884243e252e19a7a951b7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95698827"
---
# <a name="how-to-enable-and-disable-automatic-binding-redirection"></a>如何：启用和禁用自动绑定重定向

在 Visual Studio 中编译面向 .NET Framework 4.5.1 和更高版本的应用程序时，绑定重定向可能会自动添加到应用程序配置文件中，以替代程序集统一。 如果你的应用或其组件引用同一程序集的多个版本，就会添加绑定重定向，即使你在应用的配置文件中手动指定绑定重定向。 自动绑定重定向功能会影响面向 .NET Framework 4.5.1 或更高版本的桌面应用和 web 应用，但对于 web 应用而言，此行为稍有不同。 如果现有应用面向 .NET Framework 的早期版本，则可以启用自动绑定重定向，如果要手动创作绑定重定向，则可以禁用此功能。

## <a name="disable-automatic-binding-redirects-in-desktop-apps"></a>禁用桌面应用中的自动绑定重定向

默认情况下，为面向 .NET Framework 4.5.1 和更高版本的 Windows 桌面应用启用自动绑定重定向。 在编译应用程序时，绑定重定向将添加到输出配置 (**app.config**) 文件中，并重写可能发生的程序集统一。 未修改源 **app.config** 文件。 可以通过修改应用程序的项目文件，或在 Visual Studio 的项目属性中取消选中复选框来禁用此功能。

### <a name="disable-through-project-properties"></a>通过项目属性禁用

如果使用的是 Visual Studio 2017 版本15.7 或更高版本，则可以在项目的属性页中轻松禁用自动生成的绑定重定向。

1. 右键单击“解决方案资源管理器”中的项目，再选择“属性”   。

2. 在 **应用程序** 页上，取消选中 " **自动生成绑定重定向** " 选项。

3. 按 **Ctrl** + **S** 保存更改。

### <a name="disable-manually-in-the-project-file"></a>在项目文件中手动禁用

1. 使用以下方法之一打开项目文件以进行编辑：

   - 在 Visual Studio 中，选择 **解决方案资源管理器** 中的项目，然后从快捷菜单中选择 " **在文件资源管理器中打开文件夹** "。 在文件资源管理器中，找到项目 ( .csproj 或 .vbproj) 文件，并在记事本中将其打开。
   - 在 Visual Studio 的 **解决方案资源管理器** 中，右键单击项目，然后选择 " **卸载项目**"。 再次右键单击卸载的项目，然后选择 " **编辑 [项目名称 .csproj]**"。

2. 在项目文件中，查找以下属性项：

   ```xml
   <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
   ```

3. 将 `true` 更改为 `false`：

   ```xml
   <AutoGenerateBindingRedirects>false</AutoGenerateBindingRedirects>
   ```

## <a name="enable-automatic-binding-redirects-manually"></a>手动启用自动绑定重定向

你可以在面向旧版本 .NET Framework 的现有应用中启用自动绑定重定向，或在不会自动提示你添加重定向的情况下启用自动绑定重定向。 如果针对的是较新版本的框架，但不会自动提示添加重定向，则可能会获得建议重新映射程序集的生成输出。

1. 使用以下方法之一打开项目文件以进行编辑：

   - 在 Visual Studio 中，选择 **解决方案资源管理器** 中的项目，然后从快捷菜单中选择 " **在文件资源管理器中打开文件夹** "。 在文件资源管理器中，找到项目 ( .csproj 或 .vbproj) 文件，并在记事本中将其打开。
   - 在 Visual Studio 的 **解决方案资源管理器** 中，右键单击项目，然后选择 " **卸载项目**"。 再次右键单击卸载的项目，然后选择 " **编辑 [项目名称 .csproj]**"。

2. 将以下元素添加到标记) 下的第一个配置属性组 (\<PropertyGroup> ：

   ```xml
   <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
   ```

   下面显示了一个示例项目文件，其中插入了元素：

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <Project ToolsVersion="12.0" DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
     <Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" Condition="Exists('$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props')" />
     <PropertyGroup>
       <Configuration Condition=" '$(Configuration)' == '' ">Debug</Configuration>
       <Platform Condition=" '$(Platform)' == '' ">AnyCPU</Platform>
       <ProjectGuid>{123334}</ProjectGuid>
       ...
       <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
     </PropertyGroup>
     ...
   </Project>
   ```

3. 编译你的应用。

## <a name="enable-automatic-binding-redirects-in-web-apps"></a>在 web 应用中启用自动绑定重定向

Web 应用的自动绑定重定向实现方式有所不同。 由于必须为 web 应用修改源配置 (**web.config**) 文件，因此绑定重定向不会自动添加到配置文件中。 但是，Visual Studio 会通知你绑定冲突，你可以添加绑定重定向来解决此冲突。 由于始终会提示你添加绑定重定向，因此你不需要为 web 应用显式禁用此功能。

将绑定重定向添加到 **web.config** 文件：

1. 在 Visual Studio 中，编译应用，然后检查生成警告。

   ![关于程序集引用冲突的生成警告](./media/clr-assemblyrefwarning.png "CLR_AssemblyRefWarning")

2. 如果存在程序集绑定冲突，则将显示警告。 双击警告，或选择警告，然后按 **enter**。

   将显示一个对话框，通过该对话框，可以自动将所需的绑定重定向添加到源 **web.config** 文件中。

   ![绑定重定向权限对话框](./media/clr-addbindingredirect.png "CLR_AddBindingRedirect")

## <a name="see-also"></a>请参阅

- [\<bindingRedirect> 元素](./file-schema/runtime/bindingredirect-element.md)
- [重定向程序集版本](redirect-assembly-versions.md)
