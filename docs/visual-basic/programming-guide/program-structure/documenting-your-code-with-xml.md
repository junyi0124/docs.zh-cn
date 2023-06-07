---
title: 使用 XML 记录代码
ms.date: 07/20/2015
helpviewer_keywords:
- XML [Visual Basic], documenting code
- XML comments, Visual Basic
- Visual Basic code, documenting with XML
ms.assetid: a0d35dc7-c5f9-4d74-92ff-a1c6f28d5235
ms.openlocfilehash: f391fb909cfe4de8f27afb24d6db389e2c8cdfae
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590924"
---
# <a name="document-your-code-with-xml-visual-basic"></a>用 XML （Visual Basic）记录你的代码

在 Visual Basic 中，可以使用 XML 记录代码。

## <a name="xml-documentation-comments"></a>XML 文档注释

Visual Basic 提供了一种简单的方法来自动创建项目的 XML 文档。 可以为类型和成员自动生成 XML 主干，然后提供每个参数的摘要、描述性文档和其他备注。 使用适当的设置，XML 文档将自动发送到具有与项目相同的根文件名的 XML 文件中。 有关详细信息，请参阅 [-doc](../../reference/command-line-compiler/doc.md)。

可以使用 XML 文件或以其他方式将其作为 XML 来处理。 此文件与项目的输出 .exe 或 .dll 文件位于同一目录中。

XML 文档以开头 `'''` 。 处理这些注释时存在一些限制：

- 文档必须是格式正确的 XML。 如果 XML 格式不正确，则会生成警告，并且文档文件将会显示一条注释，指出遇到了错误。

- 开发人员可以随意创建自己的标记集。 建议使用一组标记（请参阅[XML 注释标记](../../language-reference/xmldoc/index.md)）。 部分建议标记具有特殊含义：

  - \<param>标记用于描述参数。 如果使用，编译器将验证该参数是否存在，以及文档是否描述了所有参数。 如果验证失败，则编译器会发出警告。

  - `cref` 属性可以附加到任何标记，以提供对代码元素的引用。 编译器验证此代码元素是否存在。 如果验证失败，则编译器会发出警告。 `Imports`查找特性中所述的类型时，编译器还会考虑所有语句 `cref` 。

  - \<summary>Visual Studio 中的 IntelliSense 使用标记显示关于类型或成员的其他信息。

## <a name="related-sections"></a>相关章节

有关创建包含文档注释的 XML 文件的详细信息，请参阅以下主题：

- [-doc](../../reference/command-line-compiler/doc.md)

- [XML 注释标记](../../language-reference/xmldoc/index.md)

- [处理 XML 文件](processing-the-xml-file.md)

- [如何：创建 XML 文档](how-to-create-xml-documentation.md)

- [Visual Studio 中的 XML 工具](/visualstudio/xml-tools/xml-tools-in-visual-studio)

## <a name="see-also"></a>另请参阅

- [使用 Visual Basic 开发应用程序](../../developing-apps/index.md)
- [Visual Basic 编程指南](../index.md)
