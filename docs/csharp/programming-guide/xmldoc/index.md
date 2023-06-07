---
title: XML 文档注释 - C# 编程指南
description: 了解 XML 文档注释。 可以通过在特殊注释字段中包含 XML 元素来为代码创建文档。
ms.date: 07/20/2015
f1_keywords:
- cs.xml
helpviewer_keywords:
- XML [C#], code comments
- comments [C#], XML
- documentation comments [C#]
- C# source code files
- C# language, XML code comments
- XML documentation comments [C#]
ms.assetid: 803b7f7b-7428-4725-b5db-9a6cff273199
ms.openlocfilehash: fbdeb53331d9fc63d24a3322ea13863d7c0a3630
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381874"
---
# <a name="xml-documentation-comments-c-programming-guide"></a>XML 文档注释（C# 编程指南）

使用 C#，可以通过以下方式为代码创建文档：将特殊注释字段中的 XML 元素包含在源代码中注释引用的代码块的前面，例如：

```csharp
/// <summary>
///  This class performs an important function.
/// </summary>
public class MyClass {}
```

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 选项进行编译时，编译器会在源代码中搜索所有 XML 标记，并创建一个 XML 文档文件。 若要基于编译器生成的文件创建最终文档，可以创建一个自定义工具，也可以使用 [DocFX](https://dotnet.github.io/docfx/) 或 [Sandcastle](https://github.com/EWSoftware/SHFB) 等工具。

若要引用 XML 元素（例如，你的函数将处理你要在 XML 文档注释中描述的特定 XML 元素），你可使用标准引用机制（`<` 和 `>`）。  若要引用代码引用 (`cref`) 元素中的通用标识符，可使用转义字符（例如，`cref="List&lt;T&gt;"`）或大括号 (`cref="List{T}"`)。  作为特例，编译器会将大括号解析为尖括号以在引用通用标识符时使作者能够更轻松地进行文档注释。

> [!NOTE]
> XML 文档注释不是元数据；它们不包括在编译的程序集中，因此无法通过反射对其进行访问。

## <a name="in-this-section"></a>本节内容

- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)

- [处理 XML 文件](./processing-the-xml-file.md)

- [文档标记分隔符](./delimiters-for-documentation-tags.md)

- [如何使用 XML 文档功能](./how-to-use-the-xml-documentation-features.md)

## <a name="related-sections"></a>相关章节

有关详细信息，请参见:

- [-doc （处理文档注释）](../../language-reference/compiler-options/doc-compiler-option.md)

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
