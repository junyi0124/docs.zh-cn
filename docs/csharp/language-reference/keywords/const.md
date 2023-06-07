---
description: const 关键字 - C# 参考
title: const 关键字 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- const_CSharpKeyword
- const
helpviewer_keywords:
- const keyword [C#]
ms.assetid: 79eb447c-117b-4418-933f-97c50aa472db
ms.openlocfilehash: 312725c3a231f0ca766d5b99bf7d9308ddd634c4
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89142084"
---
# <a name="const-c-reference"></a>const（C# 参考）

使用 `const` 关键字来声明某个常量字段或常量局部变量。 常量字段和常量局部变量不是变量并且不能修改。 常量可以为数字、布尔值、字符串或 null 引用。 不要创建常量来表示你需要随时更改的信息。 例如，不要使用常量字段来存储服务的价格、产品版本号或公司的品牌名称。 这些值会随着时间发生变化；因为编译器会传播常量，所以必须重新编译通过库编译的其他代码以查看更改。 另请参阅 [readonly](./readonly.md) 关键字。 例如：

```csharp
const int X = 0;
public const double GravitationalConstant = 6.673e-11;
private const string ProductName = "Visual C#";
```

## <a name="remarks"></a>备注

常数声明的类型指定声明引入的成员类型。 常量局部变量或常量字段的初始值设定项必须是一个可以隐式转换为目标类型的常量表达式。

常数表达式是在编译时可被完全计算的表达式。 因此，对于引用类型的常数，可能的值只能是 `string` 和 null 引用。

常数声明可以声明多个常数，例如：

```csharp
public const double X = 1.0, Y = 2.0, Z = 3.0;
```

不允许在常数声明中使用  修饰符。

常数可以参与常数表达式，如下所示：

```csharp
public const int C1 = 5;
public const int C2 = C1 + 100;
```

> [!NOTE]
> [readonly](./readonly.md) 关键字与 `const` 关键字不同。 `const` 字段只能在该字段的声明中初始化。  字段可以在声明或构造函数中初始化。 因此，根据所使用的构造函数，`readonly` 字段可能具有不同的值。 另外，虽然 `const` 字段是编译时常量，但 `readonly` 字段可用于运行时常量，如此行所示：`public static readonly uint l1 = (uint)DateTime.Now.Ticks;`

## <a name="example"></a>示例

[!code-csharp[csrefKeywordsModifiers#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#5)]

## <a name="example"></a>示例

此示例说明如何将常数用作局部变量。

[!code-csharp[csrefKeywordsModifiers#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#6)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [修饰符](index.md)
- [readonly](./readonly.md)
