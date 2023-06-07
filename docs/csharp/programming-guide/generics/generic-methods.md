---
title: 泛型方法 - C# 编程指南
description: 了解通过类型参数声明的方法（泛型方法）。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], methods
ms.assetid: 673eeea2-4b48-4faa-9c4e-2e89449221b9
ms.openlocfilehash: 195e3f11c73a17931fa6331750e2ae4ee8fd6f10
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91151462"
---
# <a name="generic-methods-c-programming-guide"></a>泛型方法（C# 编程指南）

泛型方法是通过类型参数声明的方法，如下所示：  
  
 [!code-csharp[csProgGuideGenerics#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#22)]  
  
 如下示例演示使用类型参数的 `int` 调用方法的一种方式：  
  
 [!code-csharp[csProgGuideGenerics#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#23)]  
  
 还可省略类型参数，编译器将推断类型参数。 如下 `Swap` 调用等效于之前的调用：  
  
 [!code-csharp[csProgGuideGenerics#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#24)]  
  
 类型推理的相同规则适用于静态方法和实例方法。 编译器可基于传入的方法参数推断类型参数；而无法仅根据约束或返回值推断类型参数。 因此，类型推理不适用于不具有参数的方法。 类型推理发生在编译时，之后编译器尝试解析重载的方法签名。 编译器将类型推理逻辑应用于共用同一名称的所有泛型方法。 在重载解决方案步骤中，编译器仅包含在其上类型推理成功的泛型方法。  
  
 在泛型类中，非泛型方法可访问类级别类型参数，如下所示：  
  
 [!code-csharp[csProgGuideGenerics#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#25)]  
  
 如果定义一个具有与包含类相同的类型参数的泛型方法，则编译器会生成警告 [CS0693](../../misc/cs0693.md)，因为在该方法范围内，向内 `T` 提供的参数会隐藏向外 `T` 提供的参数。 如果需要使用类型参数（而不是类实例化时提供的参数）调用泛型类方法所具备的灵活性，请考虑为此方法的类型参数提供另一标识符，如下方示例中 `GenericList2<T>` 所示。  
  
 [!code-csharp[csProgGuideGenerics#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#26)]  
  
 使用约束在方法中的类型参数上实现更多专用操作。 此版 `Swap<T>` 现名为 `SwapIfGreater<T>`，仅可用于实现 <xref:System.IComparable%601> 的类型参数。  
  
 [!code-csharp[csProgGuideGenerics#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#27)]  
  
 泛型方法可重载在数个泛型参数上。 例如，以下方法可全部位于同一类中：  
  
 [!code-csharp[csProgGuideGenerics#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#28)]  
  
## <a name="c-language-specification"></a>C# 语言规范  

 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/classes.md#methods)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Collections.Generic>
- [C# 编程指南](../index.md)
- [泛型介绍](./index.md)
- [方法](../classes-and-structs/methods.md)
