---
title: 多维数组 - C# 编程指南
description: C# 中的数组可具有多个维度。 此示例声明创建一个具有四行两列的二维数组。
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#], multidimensional
- multidimensional arrays [C#]
ms.assetid: 020ce02e-7dff-4273-8e53-bf0b33747232
ms.openlocfilehash: a2f204c0866672b317569fbc620aa8af60829ffd
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475003"
---
# <a name="multidimensional-arrays-c-programming-guide"></a>多维数组（C# 编程指南）

数组可具有多个维度。 例如，以下声明创建一个具有四行两列的二维数组。  
  
 [!code-csharp[csProgGuideArrays#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#11)]  
  
 以下声明创建一个具有三个维度（4、2 和 3）的数组。  
  
 [!code-csharp[csProgGuideArrays#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#12)]  
  
## <a name="array-initialization"></a>数组初始化

 声明后即可初始化数组，如以下示例所示。  
  
 [!code-csharp[csProgGuideArrays#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#13)]  
  
 还可在不指定级别的情况下初始化数组。  
  
 [!code-csharp[csProgGuideArrays#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#14)]  
  
 如果选择在不初始化的情况下声明数组变量，则必须使用 `new` 运算符将数组赋予变量。 `new` 的用法如以下示例所示。  
  
 [!code-csharp[csProgGuideArrays#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#15)]  
  
 以下示例将值赋予特定的数组元素。  
  
 [!code-csharp[csProgGuideArrays#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#16)]  
  
 同样，以下示例将获取特定数组元素的值并将其赋予变量 `elementValue`。  
  
 [!code-csharp[csProgGuideArrays#42](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#42)]  
  
 以下代码示例将数组元素初始化为默认值（交错数组除外）。  
  
 [!code-csharp[csProgGuideArrays#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#17)]  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [数组](./index.md)
- [一维数组](./single-dimensional-arrays.md)
- [交错数组](./jagged-arrays.md)
