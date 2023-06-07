---
title: 如何在 COM 互操作编程中使用索引属性 - C# 编程指南
description: 了解索引属性改进了在此 C# 示例中使用具有参数的 COM 属性的方式。
ms.date: 07/20/2015
helpviewer_keywords:
- indexed properties [C#]
- Office programming [C#], indexed properties
- properties [C#], indexed
ms.assetid: 756bfc1e-7c28-4d4d-b114-ac9288c73882
ms.openlocfilehash: 5f239a0772f734391bd68ef6618ea8ece8e8c9cd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178484"
---
# <a name="how-to-use-indexed-properties-in-com-interop-programming-c-programming-guide"></a>如何在 COM 互操作编程中使用索引属性（C# 编程指南）

索引属性改进了在 C# 编程中使用具有参数的 COM 属性的方式。 结合使用索引属性与 Visual C# 中的其他功能（如[命名实参和可选实参](../classes-and-structs/named-and-optional-arguments.md)、一种新类型（[动态](../../language-reference/builtin-types/reference-types.md)）以及[嵌入类型信息](../../../standard/assembly/embed-types-visual-studio.md)）可以增强 Microsoft Office 编程。  
  
 在早期版本的 C# 中，仅当 `get` 方法没有参数且 `set` 方法有且只有一个值参数时，方法才能作为属性访问。 但是，并非所有 COM 属性都符合上述限制。 例如，Excel <xref:Microsoft.Office.Interop.Excel.Range.Range%2A> 属性具有一个 `get` 访问器，它需要该范围名称的一个参数。 过去，由于无法直接访问 `Range` 属性，因此必须使用 `get_Range` 方法，如以下示例所示。  
  
 [!code-csharp[csProgGuideIndexedProperties#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#1)]  
  
 利用索引属性，你可以改为编写以下代码：  
  
 [!code-csharp[csProgGuideIndexedProperties#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#2)]  
  
> [!NOTE]
> 上一示例还使用了[可选实参](../classes-and-structs/named-and-optional-arguments.md)功能，以便忽略 `Type.Missing`。  
  
 与在 C# 3.0 及更早版本中设置 <xref:Microsoft.Office.Interop.Excel.Range> 对象的 `Value` 属性的值类似，需要两个参数。 一个为指定范围值类型的可选参数提供实参。 另一个提供 `Value` 属性的值。 下面的示例说明了这些方法。 两者都将 A1 单元格的值设置为 `Name`。
  
 [!code-csharp[csProgGuideIndexedProperties#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#3)]  
  
 利用索引属性，你可以改为编写以下代码。  
  
 [!code-csharp[csProgGuideIndexedProperties#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#4)]  
  
 你不能创建自己的索引属性。 该功能仅支持使用现有索引属性。  
  
## <a name="example"></a>示例  

 以下代码显示完整示例。 有关如何设置访问 Office API 的项目的详细信息，请参阅[如何使用 C# 功能访问 Office 互操作对象](./how-to-access-office-onterop-objects.md)。
  
 [!code-csharp[csProgGuideIndexedProperties#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguideindexedproperties/cs/program.cs#5)]  
  
## <a name="see-also"></a>请参阅

- [命名参数和可选参数](../classes-and-structs/named-and-optional-arguments.md)
- [dynamic](../../language-reference/builtin-types/reference-types.md)
- [使用类型 dynamic](../types/using-type-dynamic.md)
- [如何在 Office 编程中使用命名参数和可选参数](../classes-and-structs/how-to-use-named-and-optional-arguments-in-office-programming.md)
- [如何使用 C# 功能访问 Office 互操作对象](./how-to-access-office-onterop-objects.md)
- [演练：Office 编程](./walkthrough-office-programming.md)
