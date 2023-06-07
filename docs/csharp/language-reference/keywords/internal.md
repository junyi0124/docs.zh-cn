---
description: internal - C# 参考
title: internal - C# 参考
ms.date: 07/20/2015
f1_keywords:
- internal_CSharpKeyword
- internal
helpviewer_keywords:
- internal keyword [C#]
ms.assetid: 6ee0785c-d7c8-49b8-bb72-0a4dfbcb6461
ms.openlocfilehash: c66f4ff578e9864ebaf2b89ec03ce95f3cb2ba91
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91168733"
---
# <a name="internal-c-reference"></a>internal（C# 参考）

`internal` 关键字是类型和类型成员的[访问修饰符](./access-modifiers.md)。
  
 > 本页涵盖 `internal` 访问。 `internal` 关键字也是 [`protected internal`](./protected-internal.md) 访问修饰符的一部分。
  
只有在同一程序集的文件中，内部类型或成员才可访问，如下例所示：  
  
```csharp  
public class BaseClass
{  
    // Only accessible within the same assembly.
    internal static int x = 0;
}  
```  

 有关 `internal` 和其他访问修饰符的比较，请参阅[可访问性级别](./accessibility-levels.md)和[访问修饰符](../../programming-guide/classes-and-structs/access-modifiers.md)。  
  
 有关程序集的详细信息，请参阅 [.NET 中的程序集](../../../standard/assembly/index.md)。  
  
 内部访问通常用于基于组件的开发，因为它可使一组组件以私有方式进行协作，而不必向应用程序代码的其余部分公开。 例如，用于生成图形用户界面的框架可以提供 `Control` 和 `Form` 类，这两个类通过使用具有内部访问权限的成员进行协作。 由于这些成员是内部的，因此不会向正在使用框架的代码公开。  
  
 从定义具有内部访问权限的类型或成员的程序集外部引用该类型或成员是错误的。  
  
## <a name="example"></a>示例  

 此示例包含两个文件，即 `Assembly1.cs` 和 `Assembly1_a.cs`。 第一个文件包含内部基类 `BaseClass`。 在第二个文件中，尝试实例化 `BaseClass` 会产生错误。  
  
```csharp  
// Assembly1.cs  
// Compile with: /target:library  
internal class BaseClass
{  
   public static int intM = 0;  
}  
```  
  
```csharp  
// Assembly1_a.cs  
// Compile with: /reference:Assembly1.dll  
class TestAccess
{  
   static void Main()
   {  
      var myBase = new BaseClass();   // CS0122  
   }  
}  
```  
  
## <a name="example"></a>示例  

 在此示例中，使用在示例 1 中所用的相同文件，并将 `BaseClass` 的可访问性级别更改为 `public`。 另将成员 `intM` 的可访问性级别更改为 `internal`。 在此例中，可以实例化类，但不能访问内部成员。  
  
```csharp  
// Assembly2.cs  
// Compile with: /target:library  
public class BaseClass
{  
   internal static int intM = 0;  
}  
```  
  
```csharp  
// Assembly2_a.cs  
// Compile with: /reference:Assembly2.dll  
public class TestAccess
{  
   static void Main()
   {  
      var myBase = new BaseClass();   // Ok.  
      BaseClass.intM = 444;    // CS0117  
   }  
}  
```  
  
## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[声明的可访问性](~/_csharplang/spec/basic-concepts.md#declared-accessibility)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [访问修饰符](./access-modifiers.md)
- [可访问性级别](./accessibility-levels.md)
- [修饰符](index.md)
- [public](./public.md)
- [private](./private.md)
- [protected](./protected.md)
