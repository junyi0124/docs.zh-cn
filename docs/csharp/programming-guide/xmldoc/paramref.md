---
title: <paramref> - C# 编程指南
description: 了解 XML <paramref> 标记。 此标记提供一种指示代码中的某个词为参数的方法。
ms.date: 07/20/2015
f1_keywords:
- paramref
- <paramref>
helpviewer_keywords:
- <paramref> C# XML tag
- paramref C# XML tag
ms.assetid: 756c24c1-f591-40e8-a838-559761539b0b
ms.openlocfilehash: 133f43abfaf349806404d6d37fb472e3145c51b7
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381835"
---
# <a name="paramref-c-programming-guide"></a>\<paramref>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<paramref name="name"/>
```

## <a name="parameters"></a>参数

- `name`

  要引用的参数的名称。 用双引号 (" ") 将名称引起来。

## <a name="remarks"></a>备注

`<paramref>` 标记提供一种方式，用于指示 `<summary>` 或 `<remarks>` 块等代码注释中的单词引用某个参数。 可以处理 XML 文件以明显的方式设置此单词的格式，如使用粗体或斜体。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#7)]

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
