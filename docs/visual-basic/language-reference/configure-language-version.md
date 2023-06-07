---
title: 选择 Visual Basic 语言版本
description: 配置编译器能够执行语法验证使用的特定编译器版本。
ms.date: 05/24/2018
ms.openlocfilehash: 4768d59a37d168b2883396f1dea4d0c1a0ff4ca7
ms.sourcegitcommit: 4c41ec195caf03d98b7900007c3c8e24eba20d34
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268264"
---
# <a name="select-the-visual-basic-language-version"></a>选择 Visual Basic 语言版本

Visual Basic 编译器将默认为已发布的语言的最新主版本。 你可以选择使用该语言的新单点版本编译任何项目。 选择语言的较新版本，让你的项目可以使用最新的语言功能。 在其他情况下，可能需要在使用较旧的语言版本时验证项目的编译内容是否整齐。

借助此功能，在开发环境中安装新版本的 SDK 和工具时，不必选择在项目中引入新的语言功能。 可以在生成计算机上安装最新的 SDK 和工具。 每个项目可以配置为对其生成使用该语言的特定版本。

有三种方法设置的语言版本：

- 手动编辑你[ **.vbproj**文件](#edit-the-vbproj-file)
- 设置的语言版本[的子目录中的多个项目](#configure-multiple-projects)
- 配置[`-langversion`编译器选项](#set-the-langversion-compiler-option)

## <a name="edit-the-vbproj-file"></a>编辑 vbproj 文件

您可以设置的语言版本您 **.vbproj**文件。 添加以下元素：

```xml
<PropertyGroup>
   <LangVersion>latest</LangVersion>
</PropertyGroup>
```

值`latest`使用 Visual Basic 语言的最新次版本。 有效值为：

|值|含义|
|------------|-------------|
|default|编译器接受它可支持的最新主版本中的所有有效语言语法。|
|9|编译器接受仅包含在 Visual Basic 9.0 或更低的语法。|
|10|编译器接受仅包含在 Visual Basic 10.0 或更低的语法。|
|11|编译器接受仅包含在 Visual Basic 11.0 或更低的语法。|
|12|编译器接受仅包含在 Visual Basic 12.0 或较低的语法。|
|14|编译器接受仅包含在 Visual Basic 14.0 或更低的语法。|
|15|编译器接受仅包含在 Visual Basic 15.0 或更低的语法。|
|15.3|编译器接受仅包含在 Visual Basic 15.3 或更低的语法。|
|15.5|编译器接受仅包含在 Visual Basic 15.5 或更低的语法。|
|15.8|编译器接受仅包含在 Visual Basic 15.8 或较低的语法。|
|最新|编译器接受它可支持的所有有效语言语法。|

特殊字符串 `default` 和 `latest` 分别解析为生成计算机上安装的最新主要和次要语言版本。

## <a name="configure-multiple-projects"></a>配置多个项目

可以创建包含 `<LangVersion>` 元素的 Directory.build.props 文件来配置多个目录  。 通常是在解决方案目录中完成这件事。 将以下内容添加到解决方案目录中的 Directory.build.props 文件  ：

```xml
<Project>
 <PropertyGroup>
   <LangVersion>15.5</LangVersion>
 </PropertyGroup>
</Project>
```

现在，生成包含该文件的目录的每个子目录中将使用 Visual Basic 版本 15.5 语法。 有关详细信息，请参阅关于[自定义生成](/visualstudio/msbuild/customize-your-build)的文章。

## <a name="set-the-langversion-compiler-option"></a>选择 langversion 编译器选项

你可以使用 `-langversion` 命令行选项。 有关详细信息，请参阅关于 [/langversion](../reference/command-line-compiler/langversion.md) 编译器选项的文章。 可以通过键入看到的有效值列表`vbc -langversion:?`。
