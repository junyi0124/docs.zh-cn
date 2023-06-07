---
title: 打包和部署自定义 My 扩展
ms.date: 08/14/2018
helpviewer_keywords:
- My namespace [Visual Basic], customizing
- My namespace
- My namespace [Visual Basic], extending
ms.assetid: fd89c54b-0290-4c50-95a3-ff17d4487a21
ms.openlocfilehash: 6d2cc2b01b04b30bd3b1a4371352ded20ea8664b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411748"
---
# <a name="package-and-deploy-custom-my-extensions-visual-basic"></a>打包和部署自定义 My 扩展 (Visual Basic)

Visual Basic 提供了一种简单的方法，允许你使用 Visual Studio 模板部署自定义 `My` 命名空间扩展。 如果创建的项目模板的 `My` 扩展是新项目类型的组成部分，则可以在导出模板时将自定义 `My` 扩展代码包含在项目中。 有关导出项目模板的详细信息，请参阅[如何：创建项目模板](/visualstudio/ide/how-to-create-project-templates)。

如果自定义 `My` 扩展位于单个代码文件中，则可以将该文件作为项模板导出，用户可以将该模板添加到任何类型的 Visual Basic 项目中。 然后，你可以自定义项模板，以在 Visual Basic 项目中为自定义 `My` 扩展启用附加功能和行为。 这些功能包括：

- 允许用户从 Visual Basic 项目设计器的“My 扩展”页管理你的自定义 `My` 扩展。

- 当向项目添加对指定程序集的引用时，将自动添加自定义 `My` 扩展。

- 隐藏“添加项”对话框中的 `My` 扩展项模板，使其不包含在项目项列表中。

本主题讨论如何将自定义 `My` 扩展打包为隐藏项模板，该模板可从 Visual Basic 项目设计器的“My 扩展”页进行管理。 当向项目添加对指定程序集的引用时，还可以自动添加自定义 `My` 扩展。

## <a name="create-a-my-namespace-extension"></a>创建 My 命名空间扩展

为自定义 `My` 扩展创建部署包的第一步是将扩展创建为单个代码文件。 有关如何创建自定义 `My` 扩展的详细信息和指南，请参阅[在Visual Basic 中扩展 My 命名空间](extending-the-my-namespace.md)。

## <a name="export-a-my-namespace-extension-as-an-item-template"></a>将 My 命名空间扩展作为项模板导出

在具有包含 `My` 命名空间扩展的代码文件后，你可以将代码文件作为 Visual Studio 项模板导出。 有关如何将文件作为 Visual Studio 项模板导出的说明，请参阅[如何：创建项模板](/visualstudio/ide/how-to-create-item-templates)。

> [!NOTE]
> 如果 `My` 命名空间扩展依赖于特定程序集，则可以自定义项模板，以便在添加对该程序集的引用时自动安装 `My` 命名空间扩展。 因此，在将代码文件作为 Visual Studio 项模板导出时，你需要排除该程序集引用。

## <a name="customize-the-item-template"></a>自定义项模板

可以从 Visual Basic 项目设计器的“My 扩展”页中启用项模板管理。 当向项目添加对指定程序集的引用时，还可以启用自动添加项模板。 若要启用这些自定义项，请将名为“CustomData”的新文件添加到模板，然后将新元素添加到 .vstemplate 文件中的 XML。

### <a name="add-the-customdata-file"></a>添加 CustomData 文件

CustomData 文件是具有文件扩展名 .CustomData 并包含 XML 的文本文件（可将文件名设置为任何有意义的模板值）。 CustomData 文件中的 XML 指示，当用户使用 Visual Basic 项目设计器的“My 扩展”页时，Visual Basic 应包含 `My` 扩展。 可以选择将 <`AssemblyFullName>` 属性添加到 CustomData 文件 XML 中。 这将指示 Visual Basic 在将对特定程序集的引用添加到项目时，自动安装自定义 `My` 扩展。 可以使用任何文本编辑器或 XML 编辑器创建 CustomData 文件，然后将其添加到项模板的压缩文件夹（.zip 文件）。

例如，下面的 XML 显示 CustomData 文件的内容，当向项目添加对 Microsoft.VisualBasic.PowerPacks.Vs.dll 程序集的引用时，该文件会将模板项添加到 Visual Basic 项目的“My 扩展”文件夹。

```xml
<VBMyExtensionTemplate
    ID="Microsoft.VisualBasic.Samples.MyExtensions.MyPrinterInfo"
    Version="1.0.0.0"
    AssemblyFullName="Microsoft.VisualBasic.PowerPacks.vs"
/>
```

CustomData 文件包含一个 <`VBMyExtensionTemplate>` 元素，其属性如下表所示。

|特性|描述|
|---|---|
|`ID`|必需。 扩展的唯一标识符。 如果已将具有此 ID 的扩展添加到项目，则不会提示用户再次添加该扩展。|
|`Version`|必需。 项模板的版本号。|
|`AssemblyFullName`|可选。 程序集名称。 将对此程序集的引用添加到项目时，系统将提示用户从此项模板添加 `My` 扩展。|

### <a name="add-the-customdatasignature-element-to-the-vstemplate-file"></a>将 \<CustomDataSignature> 元素添加到 .vstemplate 文件

若要将 Visual Studio 项模板标识为 `My` 命名空间扩展，还必须修改项模板的 .vstemplate 文件。 必须向 `<TemplateData>` 元素添加一个 `<CustomDataSignature>` 元素。 `<CustomDataSignature>` 元素必须包含文本 `Microsoft.VisualBasic.MyExtension`，如以下示例所示。

```xml
<CustomDataSignature>Microsoft.VisualBasic.MyExtension</CustomDataSignature>
```

不能直接修改压缩文件夹中的文件（.zip 文件）。 必须从压缩文件夹复制 .vstemplate 文件，对其进行修改，然后将压缩文件夹中的 .vstemplate 文件替换为更新后的副本。

下面的示例演示添加了 `<CustomDataSignature>` 元素的 .vstemplate 文件的内容。

```xml
<VSTemplate Version="2.0.0" xmlns="http://schemas.microsoft.com/developer/vstemplate/2005" Type="Item">
  <TemplateData>
    <DefaultName>MyCustomExtensionModule.vb</DefaultName>
    <Name>MyPrinterInfo</Name>
    <Description>Custom My Extensions Item Template</Description>
    <ProjectType>VisualBasic</ProjectType>
    <SortOrder>10</SortOrder>
    <Icon>__TemplateIcon.ico</Icon>
    <CustomDataSignature      >Microsoft.VisualBasic.MyExtension</CustomDataSignature>
  </TemplateData>
  <TemplateContent>
    <References />
    <ProjectItem SubType="Code"
                 TargetFileName="$fileinputname$.vb"
                 ReplaceParameters="true"
     >MyCustomExtensionModule.vb</ProjectItem>
  </TemplateContent>
</VSTemplate>
```

## <a name="install-the-template"></a>安装模板

若要安装模板，可以将压缩文件夹（.zip 文件）复制到 Visual Basic 项模板文件夹。 默认情况下，用户项模板位于 *%USERPROFILE%\Documents\Visual Studio \<Version\>\Templates\ItemTemplates\Visual Basic*。 也可以将模板作为 Visual Studio 安装程序 (.vsi) 文件发布。

## <a name="see-also"></a>请参阅

- [扩展 Visual Basic 中的 My 命名空间](extending-the-my-namespace.md)
- [扩展 Visual Basic 应用程序模型](extending-the-visual-basic-application-model.md)
- [自定义 My 中可用的对象](customizing-which-objects-are-available-in-my.md)
- [“项目设计器”->“My 扩展”页](/visualstudio/ide/reference/my-extensions-page-project-designer-visual-basic)
