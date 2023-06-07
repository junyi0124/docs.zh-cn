---
title: 如何替代 ToString 方法（C# 编程指南）
description: 了解如何替代 C# 中的 ToString 方法。 每个类或结构都继承对象并获取 ToString，后者返回该对象的字符串表示形式。
ms.date: 07/20/2015
helpviewer_keywords:
- ToString method, overriding in C#
- inheritance [C#], overriding OnPaint and ToString
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 8016db69-1f19-420c-8e17-98e8bebb7749
ms.openlocfilehash: 9573e6b97383d6f422c6a2802040fb9d6db709dd
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97512738"
---
# <a name="how-to-override-the-tostring-method-c-programming-guide"></a>如何替代 ToString 方法（C# 编程指南）

C# 中的每个类或结构都可隐式继承 <xref:System.Object> 类。 因此，C# 中的每个对象都会获取 <xref:System.Object.ToString%2A> 方法，该方法返回该对象的字符串表示形式。 例如，类型为 `int` 的所有变量都有一个 `ToString` 方法，使它们可以将其内容作为字符串返回：  
  
 [!code-csharp[csProgGuideInheritance#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#37)]  
  
 创建自定义类或结构时，应替代 <xref:System.Object.ToString%2A> 方法，以向客户端代码提供有关你的类型的信息。  
  
 若要深入了解如何通过 `ToString` 方法使用格式字符串和其他类型的自定义格式设置，请参阅[格式化类型](../../../standard/base-types/formatting-types.md)。  
  
> [!IMPORTANT]
> 决定通过此方法提供信息内容时，请考虑你的类或结构是否会被不受信任的代码使用。 请务必确保不提供可能被恶意代码利用的任何信息。  
  
替代类或结构中的 `ToString` 方法：
  
1. 声明具有下列修饰符和返回类型的 `ToString` 方法：  
  
    ```csharp  
    public override string ToString(){}  
    ```  
  
2. 实现该方法，使其返回一个字符串。  
  
     下面的示例返回类的名称，但特定于该类的特定实例的数据除外。  
  
     [!code-csharp[csProgGuideInheritance#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#36)]  
  
     可以测试 `ToString` 方法，如以下代码示例所示：  
  
     [!code-csharp[csProgGuideInheritance#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#38)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IFormattable>
- [C# 编程指南](../index.md)
- [类和结构](./index.md)
- [字符串](../strings/index.md)
- [string](../../language-reference/builtin-types/reference-types.md)
- [override](../../language-reference/keywords/override.md)
- [virtual](../../language-reference/keywords/virtual.md)
- [格式设置类型](../../../standard/base-types/formatting-types.md)
