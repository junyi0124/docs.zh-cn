---
title: 委托中的变体 (C#)
description: 了解如何通过 .NET 中的变体支持将方法签名与所有委托中的委托类型进行匹配。
ms.date: 07/20/2015
ms.assetid: 19de89d2-8224-4406-8964-2965b732b890
ms.openlocfilehash: 359f7051aa2eeb5d2dc9fef3d9ccb1e4aaebfb5c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167738"
---
# <a name="variance-in-delegates-c"></a>委托中的变体 (C#)

.NET Framework 3.5 引入了变体支持，用于在 C# 中匹配所有委托的方法签名和委托类型。 这表明不仅可以将具有匹配签名的方法分配给委托，还可以将返回派生程度较大的派生类型的方法分配给委托（协变），或者如果方法所接受参数的派生类型所具有的派生程度小于委托类型指定的程度（逆变），也可将其分配给委托。 这包括泛型委托和非泛型委托。  
  
 例如，思考以下代码，该代码具有两个类和两个委托：泛型和非泛型。  
  
```csharp  
public class First { }  
public class Second : First { }  
public delegate First SampleDelegate(Second a);  
public delegate R SampleGenericDelegate<A, R>(A a);  
```  
  
 创建 `SampleDelegate` 或 `SampleGenericDelegate<A, R>` 类型的委托时，可以将以下任一方法分配给这些委托。  
  
```csharp  
// Matching signature.  
public static First ASecondRFirst(Second second)  
{ return new First(); }  
  
// The return type is more derived.  
public static Second ASecondRSecond(Second second)  
{ return new Second(); }  
  
// The argument type is less derived.  
public static First AFirstRFirst(First first)  
{ return new First(); }  
  
// The return type is more derived
// and the argument type is less derived.  
public static Second AFirstRSecond(First first)  
{ return new Second(); }  
```  
  
 以下代码示例说明了方法签名与委托类型之间的隐式转换。  
  
```csharp  
// Assigning a method with a matching signature
// to a non-generic delegate. No conversion is necessary.  
SampleDelegate dNonGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a non-generic delegate.  
// The implicit conversion is used.  
SampleDelegate dNonGenericConversion = AFirstRSecond;  
  
// Assigning a method with a matching signature to a generic delegate.  
// No conversion is necessary.  
SampleGenericDelegate<Second, First> dGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a generic delegate.  
// The implicit conversion is used.  
SampleGenericDelegate<Second, First> dGenericConversion = AFirstRSecond;  
```  
  
 有关更多示例，请参阅[在委托中使用变体 (C#)](./using-variance-in-delegates.md) 和[对 Func 和 Action 泛型委托使用变体 (C#)](./using-variance-for-func-and-action-generic-delegates.md)。  
  
## <a name="variance-in-generic-type-parameters"></a>泛型类型参数中的变体  

 在 .NET Framework 4 或更高版本中，可以启用委托之间的隐式转换，以便在具有泛型类型参数所指定的不同类型按变体的要求继承自对方时，可以将这些类型的泛型委托分配给对方。  
  
 若要启用隐式转换，必须使用 `in` 或 `out` 关键字将委托中的泛型参数显式声明为协变或逆变。  
  
 以下代码示例演示了如何创建一个具有协变泛型类型参数的委托。  
  
```csharp  
// Type T is declared covariant by using the out keyword.  
public delegate T SampleGenericDelegate <out T>();  
  
public static void Test()  
{  
    SampleGenericDelegate <String> dString = () => " ";  
  
    // You can assign delegates to each other,  
    // because the type T is declared covariant.  
    SampleGenericDelegate <Object> dObject = dString;
}  
```  
  
 如果仅使用变体支持来匹配方法签名和委托类型，且不使用 `in` 和 `out` 关键字，则可能会发现有时可以使用相同的 lambda 表达式或方法实例化委托，但不能将一个委托分配给另一个委托。  
  
 在以下代码示例中，`SampleGenericDelegate<String>` 不能显式转换为 `SampleGenericDelegate<Object>`，尽管 `String` 继承 `Object`。 可以使用 `out` 关键字标记 泛型参数 `T` 解决此问题。  
  
```csharp  
public delegate T SampleGenericDelegate<T>();  
  
public static void Test()  
{  
    SampleGenericDelegate<String> dString = () => " ";  
  
    // You can assign the dObject delegate  
    // to the same lambda expression as dString delegate  
    // because of the variance support for
    // matching method signatures with delegate types.  
    SampleGenericDelegate<Object> dObject = () => " ";  
  
    // The following statement generates a compiler error  
    // because the generic type T is not marked as covariant.  
    // SampleGenericDelegate <Object> dObject = dString;  
  
}  
```  
  
### <a name="generic-delegates-that-have-variant-type-parameters-in-net"></a>.NET 中具有变体类型参数的泛型委托

.NET Framework 4 在几个现有泛型委托中引入了泛型类型参数的变体支持：  
  
- <xref:System> 命名空间的 `Action` 委托，例如 <xref:System.Action%601> 和 <xref:System.Action%602>  
  
- <xref:System> 命名空间的 `Func` 委托，例如 <xref:System.Func%601> 和 <xref:System.Func%602>  
  
- <xref:System.Predicate%601> 委托  
  
- <xref:System.Comparison%601> 委托  
  
- <xref:System.Converter%602> 委托  
  
 有关详细信息和示例，请参阅[对 Func 和 Action 泛型委托使用变体 (C#)](./using-variance-for-func-and-action-generic-delegates.md)。  
  
### <a name="declaring-variant-type-parameters-in-generic-delegates"></a>声明泛型委托中的变体类型参数  

 如果泛型委托具有协变或逆变泛型类型参数，则该委托可被称为“变体泛型委托”。  
  
 可以使用 `out` 关键字声明泛型委托中的泛型类型参数协变。 协变类型只能用作方法返回类型，而不能用作方法参数的类型。 以下代码示例演示了如何声明协变泛型委托。  
  
```csharp  
public delegate R DCovariant<out R>();  
```  
  
 可以使用 `in` 关键字声明泛型委托中的泛型类型参数逆变。 逆变类型只能用作方法参数的类型，而不能用作方法返回类型。 以下代码示例演示了如何声明逆变泛型委托。  
  
```csharp  
public delegate void DContravariant<in A>(A a);  
```  
  
> [!IMPORTANT]
> C# 中的 `ref`、`in` 和 `out` 参数不能标记为变体。  
  
 可以在同一个委托中支持变体和协变，但这只适用于不同类型的参数。 这在下面的示例中显示。  
  
```csharp  
public delegate R DVariant<in A, out R>(A a);  
```  
  
### <a name="instantiating-and-invoking-variant-generic-delegates"></a>实例化和调用变体泛型委托  

 可以像实例化和调用固定委托一样，实例化和调用变体委托。 在以下示例中，该委托通过 lambda 表达式进行实例化。  
  
```csharp  
DVariant<String, String> dvariant = (String str) => str + " ";  
dvariant("test");  
```  
  
### <a name="combining-variant-generic-delegates"></a>合并变体泛型委托  

请勿合并变体委托。 <xref:System.Delegate.Combine%2A> 方法不支持变体委托转换，并且要求委托的类型完全相同。 这会导致在使用 <xref:System.Delegate.Combine%2A> 方法或使用 `+` 运算符合并委托时出现运行时异常，如以下代码示例中所示。  
  
```csharp  
Action<object> actObj = x => Console.WriteLine("object: {0}", x);  
Action<string> actStr = x => Console.WriteLine("string: {0}", x);  
// All of the following statements throw exceptions at run time.  
// Action<string> actCombine = actStr + actObj;  
// actStr += actObj;  
// Delegate.Combine(actStr, actObj);  
```  
  
## <a name="variance-in-generic-type-parameters-for-value-and-reference-types"></a>泛型类型参数中用于值和引用类型的变体  

 泛型类型参数的变体仅支持引用类型。 例如，`DVariant<int>` 不能隐式转换为 `DVariant<Object>` 或 `DVariant<long>`，因为整数是值类型。  
  
 以下示例演示了泛型类型参数中的变体不支持值类型。  
  
```csharp  
// The type T is covariant.  
public delegate T DVariant<out T>();  
  
// The type T is invariant.  
public delegate T DInvariant<T>();  
  
public static void Test()  
{  
    int i = 0;  
    DInvariant<int> dInt = () => i;  
    DVariant<int> dVariantInt = () => i;  
  
    // All of the following statements generate a compiler error  
    // because type variance in generic parameters is not supported  
    // for value types, even if generic type parameters are declared variant.  
    // DInvariant<Object> dObject = dInt;  
    // DInvariant<long> dLong = dInt;  
    // DVariant<Object> dVariantObject = dVariantInt;  
    // DVariant<long> dVariantLong = dVariantInt;
}  
```  
  
## <a name="see-also"></a>请参阅

- [泛型](../../../../standard/generics/index.md)
- [对 Func 和 Action 泛型委托使用变体 (C#)](./using-variance-for-func-and-action-generic-delegates.md)
- [如何合并委托（多播委托）](../../delegates/how-to-combine-delegates-multicast-delegates.md)
