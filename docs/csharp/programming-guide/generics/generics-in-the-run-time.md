---
title: 运行时中的泛型 - C# 编程指南
description: 了解运行时中的泛型类型。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], at run time
ms.assetid: 119df7e6-9ceb-49df-af36-24f8f8c0747f
ms.openlocfilehash: 1d2afd34d1a80841ba711897492cd4cd6412fa53
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91184113"
---
# <a name="generics-in-the-run-time-c-programming-guide"></a>运行时中的泛型（C# 编程指南）

泛型类型或方法编译为 Microsoft 中间语言 (MSIL) 时，它包含将其标识为具有类型参数的元数据。 如何使用泛型类型的 MSIL 根据所提供的类型参数是值类型还是引用类型而有所不同。  
  
 使用值类型作为参数首次构造泛型类型时，运行时创建专用的泛型类型，MSIL 内的适当位置替换提供的一个或多个参数。 为每个用作参数的唯一值类型一次创建专用化泛型类型。  
  
 例如，假定程序代码声明了一个由整数构造的堆栈：  
  
 [!code-csharp[csProgGuideGenerics#42](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#42)]  
  
 此时，运行时生成一个专用版 <xref:System.Collections.Generic.Stack%601> 类，其中用整数相应地替换其参数。 现在，每当程序代码使用整数堆栈时，运行时都重新使用已生成的专用 <xref:System.Collections.Generic.Stack%601> 类。 在下面的示例中创建了两个整数堆栈实例，且它们共用 `Stack<int>` 代码的一个实例：  
  
 [!code-csharp[csProgGuideGenerics#43](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#43)]  
  
 但是，假定在代码中另一点上再创建一个将不同值类型（例如 `long` 或用户定义结构）作为参数的 <xref:System.Collections.Generic.Stack%601> 类。 其结果是，运行时在 MSIL 中生成另一个版本的泛型类型并在适当位置替换 `long`。 转换已不再必要，因为每个专用化泛型类本机包含值类型。  
  
 对于引用类型，泛型的作用方式略有不同。 首次使用任意引用类型构造泛型类型时，运行时创建一个专用化泛型类型，用对象引用替换 MSIL 中的参数。 之后，每次使用引用类型作为参数实例化已构造的类型时，无论何种类型，运行时皆重新使用先前创建的专用版泛型类型。 原因可能在于所有引用大小相同。  
  
 例如，假定有两个引用类型、一个 `Customer` 类和一个 `Order` 类，并假定已创建 `Customer` 类型的堆栈：  
  
 [!code-csharp[csProgGuideGenerics#47](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#47)]  
  
 [!code-csharp[csProgGuideGenerics#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#44)]  
  
 此时，运行时生成一个专用版 <xref:System.Collections.Generic.Stack%601> 类，此类存储之后会被填写的引用类型，而不是存储数据。 假定下一行代码创建另一引用类型的堆栈，其名为 `Order`：  
  
 [!code-csharp[csProgGuideGenerics#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#45)]  
  
 不同于值类型，不会为 `Order` 类型创建 <xref:System.Collections.Generic.Stack%601> 类的另一专用版。 相反，创建专用版 <xref:System.Collections.Generic.Stack%601> 类的实例并将 `orders` 变量设置为引用此实例。 假定之后遇到一行创建 `Customer` 类型堆栈的代码：  
  
 [!code-csharp[csProgGuideGenerics#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#46)]  
  
 与之前使用通过 `Order` 类型创建的 <xref:System.Collections.Generic.Stack%601> 类一样，会创建专用 <xref:System.Collections.Generic.Stack%601> 类的另一个实例。 其中包含的指针设置为引用 `Customer` 类型大小的内存区。 由于引用类型的数量因程序不同而有较大差异，因此通过将编译器为引用类型的泛型类创建的专用类的数量减少至 1，泛型的 C# 实现可极大减少代码量。  
  
 此外，使用值类型或引用类型参数实例化泛型 C# 类时，反射可在运行时对其进行查询，且其实际类型和类型参数皆可被确定。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Generic>
- [C# 编程指南](../index.md)
- [泛型介绍](./index.md)
- [泛型](../../../standard/generics/index.md)
