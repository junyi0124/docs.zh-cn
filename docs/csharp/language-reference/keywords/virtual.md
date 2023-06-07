---
description: virtual - C# 参考
title: virtual - C# 参考
ms.date: 07/20/2015
f1_keywords:
- virtual_CSharpKeyword
- virtual
helpviewer_keywords:
- virtual keyword [C#]
ms.assetid: 5da9abae-bc1e-434f-8bea-3601b8dcb3b2
ms.openlocfilehash: 67bdfcf27bb108ca85e94ba7fdce208e4cd83b80
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89141720"
---
# <a name="virtual-c-reference"></a>virtual（C# 参考）

`virtual` 关键字用于修改方法、属性、索引器或事件声明，并使它们可以在派生类中被重写。 例如，此方法可被任何继承它的类替代：

```csharp
public virtual double Area()
{
    return x * y;
}
```

虚拟成员的实现可由派生类中的[替代成员](override.md)更改。 有关如何使用 `virtual` 关键字的详细信息，请参阅[使用 Override 和 New 关键字进行版本控制](../../programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords.md)和[了解何时使用 Override 和 New 关键字](../../programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md)。

## <a name="remarks"></a>备注

调用虚拟方法时，将为替代的成员检查该对象的运行时类型。 将调用大部分派生类中的该替代成员，如果没有派生类替代该成员，则它可能是原始成员。

默认情况下，方法是非虚拟的。 不能替代非虚方法。

`virtual` 修饰符不能与 `static`、`abstract``private` 或 `override` 修饰符一起使用。 以下示例显示了虚拟属性：

[!code-csharp[csrefKeywordsModifiers#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#26)]

除声明和调用语法不同外，虚拟属性的行为与虚拟方法相似。

- 在静态属性上使用 `virtual` 修饰符是错误的。

- 通过包括使用 `override` 修饰符的属性声明，可在派生类中替代虚拟继承属性。

## <a name="example"></a>示例

在该示例中，`Shape` 类包含 `x`、`y` 两个坐标和 `Area()` 虚拟方法。 不同的形状类（如 `Circle`、`Cylinder` 和 `Sphere`）继承 `Shape` 类，并为每个图形计算表面积。 每个派生类都有各自的 `Area()` 替代实现。

请注意，继承的类 `Circle``Sphere` 和 `Cylinder` 均使用初始化基类的构造函数，如下面的声明中所示。

```csharp
public Cylinder(double r, double h): base(r, h) {}
```

根据与方法关联的对象，下面的程序通过调用 `Area()` 方法的相应实现来计算并显示每个对象的相应区域。

[!code-csharp[csrefKeywordsModifiers#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#23)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [多态性](../../programming-guide/classes-and-structs/polymorphism.md)
- [abstract](abstract.md)
- [override](override.md)
- [new（修饰符）](new-modifier.md)
