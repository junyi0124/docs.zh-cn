---
title: 静态构造函数 - C# 编程指南
description: C# 中的静态构造函数在创建第一个实例或引用静态成员之前，只需初始化一次静态数据或执行一次操作。
ms.date: 07/20/2015
helpviewer_keywords:
- static constructors [C#]
- constructors [C#], static
ms.assetid: 151ec95e-3c4d-4ed7-885d-95b7a3be2e7d
ms.openlocfilehash: 07b3e54c9ffeb1abacaf5ddd04d2058697e653e4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203964"
---
# <a name="static-constructors-c-programming-guide"></a>静态构造函数（C# 编程指南）

静态构造函数用于初始化任何[静态](../../language-reference/keywords/static.md)数据，或执行仅需执行一次的特定操作。 将在创建第一个实例或引用任何静态成员之前自动调用静态构造函数。  
  
 [!code-csharp[csProgGuideObjects#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#14)]  

## <a name="remarks"></a>备注

静态构造函数具有以下属性：  
  
- 静态构造函数不使用访问修饰符或不具有参数。  

- 类或结构只能有一个静态构造函数。

- 静态构造函数不能继承或重载。

- 静态构造函数不能直接调用，并且仅应由公共语言运行时 (CLR) 调用。 可以自动调用它们。

- 用户无法控制在程序中执行静态构造函数的时间。
  
- 在创建第一个实例或引用任何静态成员之前，将自动调用静态构造函数以初始化[类](../../language-reference/keywords/class.md)。 静态构造函数应在实例构造函数之前运行。 调用（而不是分配）分配给事件或委托的静态方法时，将调用类型的静态构造函数。 如果静态构造函数类中存在静态字段变量初始值设定项，它们将在执行静态构造函数之前立即以在类声明中显示的文本顺序执行。

- 如果未提供静态构造函数来初始化静态字段，会将所有静态字段初始化为其默认值，如 [C# 类型的默认值](../../language-reference/builtin-types/default-values.md)中所列。
  
- 如果静态构造函数引发异常，运行时将不会再次调用该函数，并且类型在程序运行所在的应用程序域的生存期内将保持未初始化。 大多数情况下，当静态构造函数无法实例化一个类型时，或者当静态构造函数中发生未经处理的异常时，将引发 <xref:System.TypeInitializationException> 异常。 对于未在源代码中显式定义的隐式静态构造函数，故障排除可能需要检查中间语言 (IL) 代码。

- 静态构造函数的存在将防止添加 <xref:System.Reflection.TypeAttributes.BeforeFieldInit> 类型属性。 这将限制运行时优化。

- 声明为 `static readonly` 的字段可能仅被分配为其声明的一部分或在静态构造函数中。 如果不需要显式静态构造函数，请在声明时初始化静态字段，而不是通过静态构造函数实现更好的运行时优化。

> [!Note]
> 尽管不可直接访问，但应记录显式静态构造函数的存在，以帮助故障排除初始化异常。

### <a name="usage"></a>用法

- 静态构造函数的一种典型用法是在类使用日志文件且将构造函数用于将条目写入到此文件中时使用。  
- 静态构造函数对于创建非托管代码的包装类也非常有用，这种情况下构造函数可调用 `LoadLibrary` 方法。  

- 也可在静态构造函数中轻松地对无法在编译时通过约束（类型参数约束）检查的类型参数强制执行运行时检查。

## <a name="example"></a>示例

 在此示例中，类 `Bus` 具有静态构造函数。 创建 `Bus` 的第一个实例 (`bus1`) 时，将调用该静态构造函数，以便初始化类。 示例输出验证即使创建了两个 `Bus` 的实例，静态构造函数也仅运行一次，并且在实例构造函数运行前运行。  
  
 [!code-csharp[csProgGuideObjects#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#15)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[静态构造函数](~/_csharplang/spec/classes.md#static-constructors)部分。
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类和结构](./index.md)
- [构造函数](./constructors.md)
- [静态类和静态类成员](./static-classes-and-static-class-members.md)
- [终结器](./destructors.md)
- [构造函数设计准则](../../../standard/design-guidelines/constructor.md#type-constructor-guidelines)
- [安全警告 - CA2121：静态构造函数应为私有](/visualstudio/code-quality/ca2121-static-constructors-should-be-private)
