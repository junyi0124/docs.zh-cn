---
title: 指针转换 - C# 编程指南
description: 了解指针转换。 参阅隐式和显式指针转换表格、代码示例，并查看其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- pointers [C#], conversions
ms.assetid: f0e87502-477a-4ede-a31f-7a3e262e46fb
ms.openlocfilehash: 7a37c4e9f6333c00c7842df0fdaf353df516974d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91205069"
---
# <a name="pointer-conversions-c-programming-guide"></a>指针转换（C# 编程指南）

下表显示预定义隐式指针转换。 隐式转换可能会在许多情况下出现（包括方法调用和赋值语句）。  
  
## <a name="implicit-pointer-conversions"></a>隐式指针转换  
  
|From|收件人|  
|----------|--------|  
|任何指针类型|void*|  
|null|任何指针类型|  
  
 显式指针转换用于使用强制转换对不包含隐式转换的转换执行操作。 下表显示了这些转换。  
  
## <a name="explicit-pointer-conversions"></a>显式指针转换  
  
|From|收件人|  
|----------|--------|  
|任何指针类型|其他任何指针类型|  
|sbyte、byte、short、ushort、int、uint、long 或 ulong|任何指针类型|  
|任何指针类型|sbyte、byte、short、ushort、int、uint、long 或 ulong|  
  
## <a name="example"></a>示例  

 在下面的示例中，`int` 的指针将转换为 `byte` 的指针。 请注意，指针指向变量的最低寻址字节。 如果结果连续递增，直到达到 `int` 的大小（4 字节），可显示变量的其余字节。  
  
 [!code-csharp[csProgGuidePointers#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers2.cs#3)]  
  
 [!code-csharp[csProgGuidePointers#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers.cs#4)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [指针类型](pointer-types.md)
- [引用类型](../../language-reference/keywords/reference-types.md)
- [值类型](../../language-reference/builtin-types/value-types.md)
- [unsafe](../../language-reference/keywords/unsafe.md)
- [fixed 语句](../../language-reference/keywords/fixed-statement.md)
- [stackalloc](../../language-reference/operators/stackalloc.md)
