---
title: 创建嵌套组（C# 中的 LINQ）
description: 了解如何在 C# 中的 LINQ 查询表达式中创建嵌套组。
ms.date: 12/01/2016
ms.assetid: e9f00708-362e-4d13-98c5-d77549347ba0
ms.openlocfilehash: 7d056c9e215ccc7ca24d621b64e2328bed79f22e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688613"
---
# <a name="create-a-nested-group"></a>创建嵌套组

以下示例演示如何在 LINQ 查询表达式中创建嵌套组。 首先根据学生年级创建每个组，然后根据每个人的姓名进一步细分为小组。

## <a name="example"></a>示例

> [!NOTE]
> 此示例包含对[查询对象集合](query-a-collection-of-objects.md)内示例代码中所定义对象的引用。

[!code-csharp[csProgGuideLINQ#24](~/samples/snippets/csharp/concepts/linq/how-to-create-a-nested-group_1.cs)]

请注意，需要使用 3 个嵌套的 `foreach` 循环来循环访问嵌套组的内部元素。

## <a name="see-also"></a>另请参阅

- [语言集成查询 (LINQ)](index.md)
