---
title: 如何实现和调用自定义扩展方法 - C# 编程指南
description: 了解如何为任意 .NET 类型实现扩展方法。 客户端代码可通过添加对 DLL 的引用并添加 using 指令来使用方法。
ms.date: 07/20/2015
helpviewer_keywords:
- extension methods [C#], implementing and calling
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 7dab2a56-cf8e-4a47-a444-fe610a02772a
ms.openlocfilehash: 4ae48a05d451a3276b3a0f2ee4d6c633ce7db306
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513000"
---
# <a name="how-to-implement-and-call-a-custom-extension-method-c-programming-guide"></a>如何实现和调用自定义扩展方法（C# 编程指南）

本主题将介绍如何为任意 .NET 类型实现自定义扩展方法。 客户端代码可以通过以下方法使用扩展方法，添加包含这些扩展方法的 DLL 的引用，以及添加 [using](../../language-reference/keywords/using-directive.md) 指令，该指令指定在其中定义扩展方法的命名空间。  
  
## <a name="to-define-and-call-the-extension-method"></a>定义和调用扩展方法  
  
1. 定义包含扩展方法的静态[类](./static-classes-and-static-class-members.md)。  
  
     此类必须对客户端代码可见。 有关可访问性规则的详细信息，请参阅[访问修饰符](./access-modifiers.md)。  
  
2. 将扩展方法实现为静态方法，并且使其可见性至少与所在类的可见性相同。  
  
3. 此方法的第一个参数指定方法所操作的类型；此参数前面必须加上 [this](../../language-reference/keywords/this.md) 修饰符。  
  
4. 在调用代码中，添加 `using` 指令，用于指定包含扩展方法类的[命名空间](../../language-reference/keywords/namespace.md)。  
  
5. 和调用类型的实例方法那样调用这些方法。  
  
     请注意，第一个参数并不是由调用代码指定，因为它表示要在其上应用运算符的类型，并且编译器已经知道对象的类型。 你只需通过 `n` 提供形参 2 的实参。  
  
## <a name="example"></a>示例  

 以下示例实现 `CustomExtensions.StringExtension` 类中名为 `WordCount` 的扩展方法。 此方法对 <xref:System.String> 类进行操作，该类指定为第一个方法参数。 将 `CustomExtensions` 命名空间导入应用程序命名空间，并在 `Main` 方法内部调用此方法。  
  
 [!code-csharp[csProgGuideExtensionMethods#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#1)]  
  
## <a name="net-security"></a>.NET 安全性  

 扩展方法不存在特定的安全漏洞。 始终不会将扩展方法用于模拟类型的现有方法，因为为了支持类型本身定义的实例或静态方法，已解决所有名称冲突。 扩展方法无法访问扩展类中的任何隐私数据。  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [扩展方法](./extension-methods.md)
- [LINQ（语言集成查询）](../../linq/linq-in-csharp.md)
- [静态类和静态类成员](./static-classes-and-static-class-members.md)
- [受保护](../../language-reference/keywords/protected.md)
- [internal](../../language-reference/keywords/internal.md)
- [public](../../language-reference/keywords/public.md)
- [this](../../language-reference/keywords/this.md)
- [namespace](../../language-reference/keywords/namespace.md)
