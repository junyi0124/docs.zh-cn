---
title: <remarks> - C# 编程指南
description: 了解 XML <remarks> 标记。 此标记用于添加有关某个类型的信息，从而补充由以下指定的信息： <summary>.
ms.date: 07/20/2015
f1_keywords:
- remarks
- <remarks>
helpviewer_keywords:
- remarks C# XML tag
- <remarks> C# XML tag
ms.assetid: f8641391-31f3-4735-af7a-c502a5b6a251
ms.openlocfilehash: d38905d100e24158e7a1412f6be9f01a7ced2382
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381497"
---
# <a name="remarks-c-programming-guide"></a>\<remarks>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<remarks>description</remarks>
```

## <a name="parameters"></a>参数

- `Description`

  对成员的说明。

## <a name="remarks"></a>备注

`<remarks>` 标记用于添加有关某个类型的信息，从而补充由 [\<summary>](./summary.md) 指定的信息。 此信息显示在对象浏览器窗口中。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#9)]

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
