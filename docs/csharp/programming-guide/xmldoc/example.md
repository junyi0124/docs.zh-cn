---
title: <example> - C# 编程指南
description: 了解 XML <example> 标记。 借助此标记，可以指定如何使用方法或其他库成员的示例。
ms.date: 07/20/2015
f1_keywords:
- <example>
- example
helpviewer_keywords:
- <example> C# XML tag
- example C# XML tag
ms.assetid: 32d6e73b-2554-4abb-83ee-a1e321334fd2
ms.openlocfilehash: dd529e8f2a54cf9086d0d8c555dd1adb70b99126
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381523"
---
# <a name="example-c-programming-guide"></a>\<example>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<example>description</example>
```

## <a name="parameters"></a>参数

- `description`

  代码示例的说明。

## <a name="remarks"></a>备注

借助 `<example>` 标记，可以指定如何使用方法或其他库成员的示例。 这通常涉及到使用 [\<code>](./code.md) 标记。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
