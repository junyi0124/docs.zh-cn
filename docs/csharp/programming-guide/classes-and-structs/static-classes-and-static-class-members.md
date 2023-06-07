---
title: 静态类和静态类成员 - C# 编程指南
description: 使用 C# 不可实例化静态类。 可以使用类名本身访问静态类的成员。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, static members
- static members [C#]
- static classes [C#]
- C# language, static classes
- static class members [C#]
ms.assetid: 235614b5-1371-4dbd-9abd-b406a8b0298b
ms.openlocfilehash: cbfe4b63dc27cf0a0b6aad87c4f011151bacd4e5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91199011"
---
# <a name="static-classes-and-static-class-members-c-programming-guide"></a>静态类和静态类成员（C# 编程指南）

[静态](../../language-reference/keywords/static.md)类基本上与非静态类相同，但存在一个差异：静态类无法实例化。 换句话说，无法使用 [new](../../language-reference/operators/new-operator.md) 运算符创建类类型的变量。 由于不存在任何实例变量，因此可以使用类名本身访问静态类的成员。 例如，如果你具有一个静态类，该类名为 `UtilityClass`，并且具有一个名为 `MethodA` 的公共静态方法，如下面的示例所示：  
  
```csharp  
UtilityClass.MethodA();  
```  
  
 静态类可以用作只对输入参数进行操作并且不必获取或设置任何内部实例字段的方法集的方便容器。 例如，在 .NET 类库中，静态 <xref:System.Math?displayProperty=nameWithType> 类包含执行数学运算，而无需存储或检索对 <xref:System.Math> 类特定实例唯一的数据的方法。 即，通过指定类名和方法名称来应用类的成员，如下面的示例所示。  
  
```csharp  
double dub = -3.14;  
Console.WriteLine(Math.Abs(dub));  
Console.WriteLine(Math.Floor(dub));  
Console.WriteLine(Math.Round(Math.Abs(dub)));  
  
// Output:  
// 3.14  
// -4  
// 3  
```  
  
 与所有类类型的情况一样，加载引用该类的程序时，.NET 运行时会加载静态类的类型信息。 程序无法确切指定类加载的时间。 但是，可保证进行加载，以及在程序中首次引用类之前初始化其字段并调用其静态构造函数。 静态构造函数只调用一次，在程序所驻留的应用程序域的生存期内，静态类会保留在内存中。  
  
> [!NOTE]
> 若要创建仅允许创建本身的一个实例的非静态类，请参阅[在 C# 中实现单一实例](/previous-versions/msp-n-p/ff650316(v=pandp.10))。  
  
 以下列表提供静态类的主要功能：  
  
- 只包含静态成员。  
  
- 无法进行实例化。  
  
- 会进行密封。  
  
- 不能包含[实例构造函数](./instance-constructors.md)。  
  
 因此，创建静态类基本上与创建只包含静态成员和私有构造函数的类相同。 私有构造函数可防止类进行实例化。 使用静态类的优点是编译器可以进行检查，以确保不会意外地添加任何实例成员。 编译器可保证无法创建此类的实例。  
  
 静态类会进行密封，因此不能继承。 它们不能继承自任何类（除了 <xref:System.Object>）。 静态类不能包含实例构造函数。 但是，它们可以包含静态构造函数。 如果非静态类包含了需要进行有意义的初始化的静态成员，则它也应该定义一个静态构造器。 有关详细信息，请参阅[静态构造函数](./static-constructors.md)。  
  
## <a name="example"></a>示例  

 下面是静态类的示例，该类包含将温度从摄氏度从华氏度以及从华氏度转换为摄氏度的两个方法：  
  
 [!code-csharp[csProgGuideObjects#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#31)]  
  
## <a name="static-members"></a>静态成员  

 非静态类可以包含静态方法、字段、属性或事件。 即使未创建类的任何实例，也可对类调用静态成员。 静态成员始终按类名（而不是实例名称）进行访问。 静态成员只有一个副本存在（与创建的类的实例数无关）。 静态方法和属性无法在其包含类型中访问非静态字段和事件，它们无法访问任何对象的实例变量，除非在方法参数中显式传递它。  
  
 更典型的做法是声明具有一些静态成员的非静态类（而不是将整个类都声明为静态）。 静态字段的两个常见用途是保留已实例化的对象数的计数，或是存储必须在所有实例间共享的值。  
  
 静态方法可以进行重载，但不能进行替代，因为它们属于类，而不属于类的任何实例。  
  
 虽然字段不能声明为 `static const`，不过 [const](../../language-reference/keywords/const.md) 字段在其行为方面本质上是静态的。 它属于类型，而不属于类型的实例。 因此，可以使用用于静态字段的相同 `ClassName.MemberName` 表示法来访问 `const` 字段。 无需进行对象实例化。  
  
 C# 不支持静态局部变量（即在方法范围中声明的变量）。  
  
 可在成员的返回类型之前使用 `static` 关键字声明静态类成员，如下面的示例所示：  
  
 [!code-csharp[csProgGuideObjects#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#29)]  
  
 在首次访问静态成员之前以及在调用构造函数（如果有）之前，会初始化静态成员。 若要访问静态类成员，请使用类的名称（而不是变量名称）指定成员的位置，如下面的示例所示：  
  
 [!code-csharp[csProgGuideObjects#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#30)]  
  
 如果类包含静态字段，则提供在类加载时初始化它们的静态构造函数。  
  
 对静态方法的调用会采用 Microsoft 中间语言 (MSIL) 生成调用指令，而对实例方法的调用会生成 `callvirt` 指令，该指令还会检查是否存在 null 对象引用。 但是在大多数时候，两者之间的性能差异并不显著。  
  
## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[静态类](~/_csharplang/spec/classes.md#static-classes)和[静态和实例成员](~/_csharplang/spec/classes.md#static-and-instance-members)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [static](../../language-reference/keywords/static.md)
- [类](./classes.md)
- [class](../../language-reference/keywords/class.md)
- [静态构造函数](./static-constructors.md)
- [实例构造函数](./instance-constructors.md)
