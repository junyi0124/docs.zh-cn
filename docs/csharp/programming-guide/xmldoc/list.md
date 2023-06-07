---
title: <list> - C# 编程指南
description: 了解 XML <list> 标记。 此标记用于通过使用“item”块创建表和定义、项目符号列表或编号列表。
ms.date: 07/20/2015
f1_keywords:
- list
- <list>
helpviewer_keywords:
- list C# XML tag
- listheader C# XML tag
- <listheader> C# XML tag
- item C# XML tag
- <item> C# XML tag
- <list> C# XML tag
ms.assetid: c9620b1b-c2e6-43f1-ab88-8ab47308ffec
ms.openlocfilehash: 361c2e6f343554a9b8519c3b2e41219b209e682d
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381861"
---
# <a name="list-c-programming-guide"></a>\<list>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<list type="bullet|number|table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

## <a name="parameters"></a>参数

- `term`

  要定义的术语，将在 `description` 中进行定义。

- `description`

  项目符号或编号列表中的项或 `term` 的定义。
  
## <a name="remarks"></a>备注

`<listheader>` 块用于定义表或定义列表的标题行。 定义表时，只需提供标题中的术语的项。

列表中的每个项均使用 `<item>` 块指定。 创建定义列表时，需要同时指定 `term` 和 `description`。 但是，对于表、项目符号列表或编号列表，只需提供 `description` 的项。

列表或表可根据需要具有多个 `<item>` 块。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#6)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
