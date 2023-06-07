---
title: 将数组作为参数传递 - C# 编程指南
description: C# 中的数组可以作为实参传递给方法形参。 由于数组是引用类型，因此方法可以更改元素的值。
ms.date: 07/05/2018
helpviewer_keywords:
- arrays [C#], passing as arguments
ms.assetid: f3a0971e-c87c-4a1f-8262-bc0a3b712772
ms.openlocfilehash: 68f174421e56e2cf082fe670f93c4f6627d7c17b
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474626"
---
# <a name="passing-arrays-as-arguments-c-programming-guide"></a>将数组作为参数传递（C# 编程指南）

数组可以作为实参传递给方法形参。 由于数组是引用类型，因此方法可以更改元素的值。

## <a name="passing-single-dimensional-arrays-as-arguments"></a>将一维数组作为参数传递

可将初始化的一维数组传递给方法。 例如，下列语句将一个数组发送给了 Print 方法。

[!code-csharp[csProgGuideArrays#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#34)]

下面的代码演示 Print 方法的部分实现。

[!code-csharp[csProgGuideArrays#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#33)]

可在同一步骤中初始化并传递新数组，如下例所示。

[!code-csharp[CsProgGuideArrays#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#35)]

### <a name="example"></a>示例

在下面的示例中，先初始化一个字符串数组，然后将其作为参数传递给字符串的 `DisplayArray` 方法。 该方法将显示数组的元素。 接下来，`ChangeArray` 方法会反转数组元素，然后由 `ChangeArrayElements` 方法修改该数组的前三个元素。 每个方法返回后，`DisplayArray` 方法会显示按值传递数组不会阻止对数组元素的更改。

[!code-csharp[csProgGuideArrays#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/ArrayExample.cs)]

## <a name="passing-multidimensional-arrays-as-arguments"></a>将多维数组作为参数传递

通过与传递一维数组相同的方式，向方法传递初始化的多维数组。

[!code-csharp[csProgGuideArrays#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#41)]

下列代码演示了 Print 方法的部分声明（该方法接受将二维数组作为其参数）。

[!code-csharp[csProgGuideArrays#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#36)]

可在一个步骤中初始化并传递新数组，如下例所示：

[!code-csharp[csProgGuideArrays#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#32)]

### <a name="example"></a>示例

在下列示例中，初始化一个整数的二维数组，并将其传递至 `Print2DArray` 方法。 该方法将显示数组的元素。

[!code-csharp[csProgGuideArrays#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#31)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [数组](index.md)
- [一维数组](single-dimensional-arrays.md)
- [多维数组](multidimensional-arrays.md)
- [交错数组](jagged-arrays.md)
