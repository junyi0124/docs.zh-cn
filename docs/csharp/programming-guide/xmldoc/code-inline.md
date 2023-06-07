---
title: <c> - C# 编程指南
description: 了解 XML <c> 标记。 此标记将说明中的单行文本标记为代码，而 <code> indicates multiple lines.
ms.date: 07/20/2015
f1_keywords:
- c
- <c>
helpviewer_keywords:
- text, marking as code [C#]
- code, marking text as [C#]
- c C# XML tag
- <c> C# XML tag
ms.assetid: aad5b16e-a29e-445e-bd0d-eea0b138d7b2
ms.openlocfilehash: 78e59e1df4b096782e0a97b6d12c21c843a1cb21
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382017"
---
# <a name="c-c-programming-guide"></a>\<c>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<c>text</c>
```

## <a name="parameters"></a>参数

- `text`

  要指示为代码的文本。

## <a name="remarks"></a>备注

使用 `<c>` 标记可以指示应将说明内的文本标记为代码。 使用 [\<code>](./code.md) 指示作为代码的多行文本。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#2)]
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
