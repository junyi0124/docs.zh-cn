---
title: <value> - C# 编程指南
description: 了解 XML <value> 标记。 此标记可用于描述属性表示的值。
ms.date: 07/20/2015
f1_keywords:
- <value>
helpviewer_keywords:
- <value> C# XML tag
- value C# XML tag
ms.assetid: 08dbadaf-9ab6-43d9-9493-98e43bed199a
ms.openlocfilehash: d8294b477d7067653c71d1ec2047a85a0bfe6d02
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380769"
---
# <a name="value-c-programming-guide"></a>\<value>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<value>property-description</value>
```

## <a name="parameters"></a>参数

- `property-description`

  属性的说明。

## <a name="remarks"></a>备注

`<value>` 标记可用于描述属性表示的值。 在 Visual Studio .NET 开发环境中通过代码向导添加属性时，将添加新属性的 [\<summary>](./summary.md) 标记。 然后，应手动添加 `<value>` 标记，以描述属性表示的值。

使用 [-doc](../../language-reference/compiler-options/doc-compiler-option.md) 进行编译以便将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#14)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
