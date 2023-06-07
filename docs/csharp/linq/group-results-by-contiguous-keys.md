---
title: 按连续键对结果进行分组（C# 中的 LINQ）
description: 如何使用 C# 中的 LINQ 按连续键对结果进行分组。
ms.date: 08/14/2018
ms.assetid: cbda9c08-151b-4c9e-82f7-c3d7f3dac66b
ms.openlocfilehash: b5753c85bb07be4fc84b78a299eece961969ff9d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659899"
---
# <a name="group-results-by-contiguous-keys"></a>按连续键对结果进行分组

下面的示例演示如何将元素分组为表示连续键子序列的区块。 例如，假设给定下列键值对的序列：

|键|“值”|
|---------|-----------|
|包含当前请求的 URL 的|We|
|包含当前请求的 URL 的|think|
|包含当前请求的 URL 的|that|
|B|Linq|
|C|is|
|包含当前请求的 URL 的|really|
|B|cool|
|B|!|

以下组将按此顺序创建：

1. We, think, that

2. Linq

3. is

4. really

5. cool, !

此解决方案是以扩展方法实现的，该扩展方法是线程安全的，并以流的方式返回其结果。 换言之，它在源序列中遍历移动时生成其组。 与 `group` 或 `orderby` 运算符不同，它能在所有序列被读取之前开始将组返回给调用方。

线程安全通过在循环访问源序列的同时创建每个组或区块的副本来实现，如源代码注释中所述。 如果源序列具有大型的连续项序列，则公共语言运行时可能会引发 <xref:System.OutOfMemoryException>。

## <a name="example"></a>示例

下面的示例演示该扩展方法以及使用它的客户端代码：

[!code-csharp[cscsrefContiguousGroups#1](~/samples/snippets/csharp/concepts/linq/how-to-group-results-by-contiguous-keys_1.cs)]

若要在项目中使用扩展方法，请将 `MyExtensions` 静态类复制到新的或现有源代码文件中，并且如有需要，请为它所在的命名空间添加 `using` 指令。

## <a name="see-also"></a>另请参阅

- [语言集成查询 (LINQ)](index.md)
