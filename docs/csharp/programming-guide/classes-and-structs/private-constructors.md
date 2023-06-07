---
title: 专用构造函数 - C# 编程指南
description: 私有构造函数是 C# 中的一种特殊实例构造函数，用于限制创建对象的方式。 它们可用于工厂方法或其他构造惯例。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, private constructors
- private constructors [C#]
ms.assetid: 29eeaa7d-8d81-453c-94b9-0e2800172621
ms.openlocfilehash: c6048424128b462bfc56d9c7c3cf8f75cca9298d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91159340"
---
# <a name="private-constructors-c-programming-guide"></a>私有构造函数（C# 编程指南）

私有构造函数是一种特殊的实例构造函数。 它通常用于只包含静态成员的类中。 如果类具有一个或多个私有构造函数而没有公共构造函数，则其他类（除嵌套类外）无法创建该类的实例。 例如：  
  
 [!code-csharp[csProgGuideObjects#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#11)]  
  
 声明空构造函数可阻止自动生成无参数构造函数。 请注意，如果不对构造函数使用访问修饰符，则在默认情况下它仍为私有构造函数。 但是，通常会显式地使用 [private](../../language-reference/keywords/private.md) 修饰符来清楚地表明该类不能被实例化。  
  
 当没有实例字段或实例方法（例如 <xref:System.Math> 类）时或者当调用方法以获得类的实例时，私有构造函数可用于阻止创建类的实例。 如果类中的所有方法都是静态的，可考虑使整个类成为静态的。 有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。  
  
## <a name="example"></a>示例  

 下面是使用私有构造函数的类的示例。  
  
 [!code-csharp[csProgGuideObjects#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#12)]  
  
 请注意，如果取消注释该示例中的以下语句，它将生成一个错误，因为该构造函数受其保护级别的限制而不可访问：  
  
 [!code-csharp[csProgGuideObjects#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#13)]  
  
## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[私有构造函数](~/_csharplang/spec/classes.md#private-constructors)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类和结构](./index.md)
- [构造函数](./constructors.md)
- [终结器](./destructors.md)
- [private](../../language-reference/keywords/private.md)
- [public](../../language-reference/keywords/public.md)
