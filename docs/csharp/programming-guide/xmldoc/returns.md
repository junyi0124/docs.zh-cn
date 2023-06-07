---
title: <returns> - C# 编程指南
description: 了解 XML <returns> 标记。 在方法声明的注释中使用了此标记来描述返回值。
ms.date: 07/20/2015
f1_keywords:
- returns
- <returns>
helpviewer_keywords:
- <returns> C# XML tag
- returns C# XML tag
ms.assetid: bb2d9958-62fc-47c7-9511-6311171f119f
ms.openlocfilehash: e461d46df619a351048ae7942e59847d39e490f9
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381393"
---
# <a name="returns-c-programming-guide"></a>\<returns>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<returns>description</returns>
```

## <a name="parameters"></a>参数

- `description`

  返回值的说明。

## <a name="remarks"></a>备注

在方法声明的注释中应使用 `<returns>` 标记来描述返回值。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#10)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
