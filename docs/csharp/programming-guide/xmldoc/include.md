---
title: <include> - C# 编程指南
description: 了解 XML <include> 标记。 通过此标记，可在其他文件中引用描述源代码中类型和成员的注释。
ms.date: 07/20/2015
f1_keywords:
- include
- <include>
helpviewer_keywords:
- <include> C# XML tag
- include C# XML tag
ms.assetid: a8a70302-6196-4643-bd09-ef33f411f18f
ms.openlocfilehash: d2de8fea17850685668766bc4ec6e64b1be77cce
ms.sourcegitcommit: 2560a355c76b0a04cba0d34da870df9ad94ceca3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053186"
---
# <a name="include-c-programming-guide"></a>\<include>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<include file='filename' path='tagpath[@name="id"]' />
```

## <a name="parameters"></a>参数

- `filename`

  包含文档的 XML 文件的名称。 可使用相对于源代码文件的路径限定文件名。 使用单引号 (' ') 将 `filename` 括起来。

- `tagpath`

  `filename` 中标记的路径，它指向标记 `name`。 使用单引号 (' ') 将路径括起来。

- `name`

  标记中的名称说明符（位于注释之前）；`name` 将有 `id`。

- `id`

  标记的 ID（位于注释之前）。 用双引号 (" ") 将 ID 括起来。

## <a name="remarks"></a>备注

通过 `<include>` 标记，可在其他文件中引用描述源代码中类型和成员的注释。 这是对直接在源代码文件中放入文档注释的替代方法。 通过将文档放入不同文件，可以单独从源代码对文档应用源控件。 一人可以签出源代码文件，而其他人可以签出文档文件。

`<include>` 标记使用 XML XPath 语法。 有关如何自定义 `<include>` 用法的信息，请参阅 XPath 文档。

## <a name="example"></a>示例

这是多文件示例。 下面是第一个文件，使用 `<include>`。

[!code-csharp[csProgGuideDocComments#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#5)]

第二个文件是 xml_include_tag.doc，包含下列文档注释。

```xml
<MyDocs>

<MyMembers name="test">
<summary>
The summary for this type.
</summary>
</MyMembers>

<MyMembers name="test2">
<summary>
The summary for this other type.
</summary>
</MyMembers>

</MyDocs>
```

## <a name="program-output"></a>程序输出

使用以下命令行编译 Test 和 Test2 类时，便会生成下面的输出：`-doc:DocFileName.xml.`。在 Visual Studio 的项目设计器的“生成”窗格中，指定 XML 文档注释选项。 当 C# 编译器发现 `<include>` 标记时，它将在 xml_include_tag.doc（而不是当前源文件）中搜索文档注释。 然后编译器生成 *DocFileName.xml*，这是由文档工具（如 [Sandcastle](https://github.com/EWSoftware/SHFB)）所使用的文件，用于生成最终文档。  
  
```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>xml_include_tag</name>
    </assembly>
    <members>
        <member name="T:Test">
            <summary>
The summary for this type.
</summary>
        </member>
        <member name="T:Test2">
            <summary>
The summary for this other type.
</summary>
        </member>
    </members>
</doc>
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
