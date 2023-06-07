---
title: 内置引用类型 - C# 引用
description: 了解具有 C# 关键字的引用类型，可使用这些关键字进行声明。
ms.date: 06/25/2019
f1_keywords:
- object_CSharpKeyword
- object
- delegate_CSharpKeyword
- delegate
- dynamic_CSharpKeyword
- string
- string_CSharpKeyword
helpviewer_keywords:
- object keyword [C#]
- delegate keyword [C#]
- function pointers [C#]
- dynamic [C#]
- dynamic keyword [C#]
- strings [C#], reference
- '@ string literal'
- string literals [C#]
- string keyword [C#]
ms.openlocfilehash: c2c03f47babd9ccf87eb60d33b9d65d1a9c82e2e
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223519"
---
# <a name="built-in-reference-types-c-reference"></a>内置引用类型（C# 引用）

C# 具有多个内置引用类型。 这些类型包含的关键字或运算符是 .NET 库中的类型的同义词。

## <a name="the-object-type"></a>对象类型

`object` 类型是 <xref:System.Object?displayProperty=nameWithType> 在 .NET 中的别名。 在 C# 的统一类型系统中，所有类型（预定义类型、用户定义类型、引用类型和值类型）都是直接或间接从 <xref:System.Object?displayProperty=nameWithType> 继承的。 可以将任何类型的值赋给 `object` 类型的变量。 可以使用文本 `null` 将任何 `object` 变量赋值给其默认值。 将值类型的变量转换为对象的过程称为 *装箱* 。 将 `object` 类型的变量转换为值类型的过程称为取消装箱  。 有关详细信息，请参阅[装箱和取消装箱](../../programming-guide/types/boxing-and-unboxing.md)。

## <a name="the-string-type"></a>字符串类型

`string` 类型表示零个或多个 Unicode 字符的序列。 `string` 是 <xref:System.String?displayProperty=nameWithType> 在 .NET 中的别名。

尽管 `string` 为引用类型，但是定义[相等运算符 `==` 和 `!=`](../operators/equality-operators.md#string-equality) 是为了比较 `string` 对象（而不是引用）的值。 这使得对字符串相等性的测试更为直观。 例如： 。

```csharp-interactive
string a = "hello";
string b = "h";
// Append to contents of 'b'
b += "ello";
Console.WriteLine(a == b);
Console.WriteLine(object.ReferenceEquals(a, b));
```

此时将显示“True”，然后显示“False”，因为字符串的内容是相等的，但 `a` 和 `b` 并不指代同一字符串实例。

[+ 运算符](../operators/addition-operator.md#string-concatenation)连接字符串：

```csharp
string a = "good " + "morning";
```

这将创建包含“good morning”的字符串对象。

字符串是不可变的  ，即：字符串对象在创建后，尽管从语法上看似乎可以更改其内容，但事实上并不可行。 例如，编写此代码时，编译器实际上会创建一个新的字符串对象来保存新的字符序列，且该新对象将赋给 `b`。 已为 `b` 分配的内存（当它包含字符串“h”时）可用于垃圾回收。

```csharp
string b = "h";
b += "ello";
```

`[]` [运算符](../operators/member-access-operators.md#indexer-operator-)可用于只读访问字符串的个别字符。 有效索引于 `0` 开始，且必须小于字符串的长度：

```csharp
string str = "test";
char x = str[2];  // x = 's';
```

同样，`[]` 运算符也可用于循环访问字符串中的每个字符：

```csharp-interactive
string str = "test";

for (int i = 0; i < str.Length; i++)
{
  Console.Write(str[i] + " ");
}
// Output: t e s t
```

字符串文本属于 `string` 类型且可以两种形式编写（带引号和仅 `@` 带引号）。 带引号字符串括在双引号 (") 内。

```csharp
"good morning"  // a string literal
```

字符串文本可包含任何字符文本。 包括转义序列。 下面的示例使用转义序列 `\\` 表示反斜杠，使用 `\u0066` 表示字母 f，以及使用 `\n` 表示换行符。

```csharp-interactive
string a = "\\\u0066\n F";
Console.WriteLine(a);
// Output:
// \f
//  F
```

> [!NOTE]
> 转义码 `\udddd`（其中 `dddd` 是一个四位数字）表示 Unicode 字符 U+`dddd`。 另外，还可识别八位 Unicode 转义码：`\Udddddddd`。

[逐字字符串文本](../tokens/verbatim.md)以 `@` 开头，并且也括在双引号内。 例如： 。

```csharp
@"good morning"  // a string literal
```

逐字字符串的优点是不处理转义序列，这样就可轻松编写完全限定的 Windows 文件名等  ：

```csharp
@"c:\Docs\Source\a.txt"  // rather than "c:\\Docs\\Source\\a.txt"
```

若要在 @-quoted 字符串中包含双引号，双倍添加即可：

```csharp
@"""Ahoy!"" cried the captain." // "Ahoy!" cried the captain.
```

## <a name="the-delegate-type"></a>委托类型

委托类型的声明与方法签名相似。 它有一个返回值和任意数目任意类型的参数：

```csharp
public delegate void MessageDelegate(string message);
public delegate int AnotherDelegate(MyType m, long num);
```

在 .NET 中，`System.Action` 和 `System.Func` 类型为许多常见委托提供泛型定义。 可能不需要定义新的自定义委托类型。 相反，可以创建提供的泛型类型的实例化。

`delegate` 是一种可用于封装命名方法或匿名方法的引用类型。 委托类似于 C++ 中的函数指针；但是，委托是类型安全和可靠的。 有关委托的应用，请参阅[委托](../../programming-guide/delegates/index.md)和[泛型委托](../../programming-guide/generics/generic-delegates.md)。 委托是[事件](../../programming-guide/events/index.md)的基础。 通过将委托与命名方法或匿名方法关联，可以实例化委托。

必须使用具有兼容返回类型和输入参数的方法或 lambda 表达式实例化委托。 有关方法签名中允许的差异程度的详细信息，请参阅[委托中的变体](../../programming-guide/concepts/covariance-contravariance/using-variance-in-delegates.md)。 为了与匿名方法一起使用，委托和与之关联的代码必须一起声明。

## <a name="the-dynamic-type"></a>动态类型

`dynamic` 类型表示变量的使用和对其成员的引用绕过编译时类型检查。 改为在运行时解析这些操作。 `dynamic` 类型简化了对 COM API（例如 Office Automation API）、动态 API（例如 IronPython 库）和 HTML 文档对象模型 (DOM) 的访问。

在大多数情况下，`dynamic` 类型与 `object` 类型的行为类似。 具体而言，任何非 Null 表达式都可以转换为 `dynamic` 类型。 `dynamic` 类型与 `object` 的不同之处在于，编译器不会对包含类型 `dynamic` 的表达式的操作进行解析或类型检查。 编译器将有关该操作信息打包在一起，之后这些信息会用于在运行时评估操作。 在此过程中，`dynamic` 类型的变量会编译为 `object` 类型的变量。 因此，`dynamic` 类型只在编译时存在，在运行时则不存在。

下面的示例将 `dynamic` 类型的变量与 `object` 类型的变量进行对比。 若要在编译时验证每个变量的类型，请将鼠标指针放在 `WriteLine` 语句中的 `dyn` 或 `obj` 上。 请将下面的代码复制到可以使用 IntelliSense 的编辑器中。 IntelliSense 对 `dyn` 显示“dynamic”  ，对 `obj` 显示“object”  。

[!code-csharp[csrefKeywordsTypes#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic1.cs#21)]

<xref:System.Console.WriteLine%2A> 语句显示 `dyn` 和 `obj` 的运行时类型。 此时，两者的类型均为整数。 将生成以下输出：

```console
System.Int32
System.Int32
```

若要查看编译时 `dyn` 与 `obj` 之间的区别，请在前面示例的声明和 `WriteLine` 语句之间添加下列两行。

```csharp
dyn = dyn + 3;
obj = obj + 3;
```

 尝试在表达式 `obj + 3` 中添加整数和对象时，将报告编译器错误。 但是，对于 `dyn + 3`，不会报告任何错误。 在编译时不会检查包含 `dyn` 的表达式，原因是 `dyn` 的类型为 `dynamic`。

下面的示例在多个声明中使用 `dynamic`。 `Main` 方法也将编译时类型检查与运行时类型检查进行了对比。

[!code-csharp[csrefKeywordsTypes#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic2.cs#25)]

### <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 关键字](../keywords/index.md)
- [事件](../../programming-guide/events/index.md)
- [使用类型 dynamic](../../programming-guide/types/using-type-dynamic.md)
- [有关使用字符串的最佳做法](../../../standard/base-types/best-practices-strings.md)
- [基本字符串操作](../../../standard/base-types/basic-string-operations.md)
- [新建字符串](../../../standard/base-types/creating-new.md)
- [类型测试和强制转换运算符](../operators/type-testing-and-cast.md)
- [如何使用模式匹配以及 is 和 as 运算符安全地进行强制转换](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)
- [演练：创建和使用动态对象](../../programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
- <xref:System.Object?displayProperty=nameWithType>
- <xref:System.String?displayProperty=nameWithType>
- <xref:System.Dynamic.DynamicObject?displayProperty=nameWithType>
