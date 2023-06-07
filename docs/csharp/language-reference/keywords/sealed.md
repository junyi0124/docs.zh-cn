---
description: sealed 修饰符 - C# 参考
title: sealed 修饰符 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- sealed
- sealed_CSharpKeyword
helpviewer_keywords:
- sealed keyword [C#]
ms.assetid: 8e4ed5d3-10be-47db-9488-0da2008e6f3f
ms.openlocfilehash: 5b945503c6f6546f571a2422ae77760da0363b08
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89136962"
---
# <a name="sealed-c-reference"></a>sealed（C# 参考）

应用于某个类时，`sealed` 修饰符可阻止其他类继承自该类。 在下面的示例中，类 `B` 继承自类 `A`，但没有类可以继承自类 `B`。

```csharp
class A {}
sealed class B : A {}
```

还可以对替代基类中的虚方法或属性的方法或属性使用 `sealed` 修饰符。 这使你可以允许类派生自你的类并防止它们替代特定虚方法或属性。

## <a name="example"></a>示例

在下面的示例中，`Z` 继承自 `Y`，但 `Z` 无法替代在 `X` 中声明并在 `Y` 中密封的虚函数 `F`。

[!code-csharp[csrefKeywordsModifiers#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#16)]

在类中定义新方法或属性时，可以通过不将它们声明为[虚拟](virtual.md)，来防止派生类替代它们。

将 [abstract](abstract.md) 修饰符与密封类结合使用是错误的，因为抽象类必须由提供抽象方法或属性的实现的类来继承。

应用于方法或属性时，`sealed` 修饰符必须始终与 [override](override.md) 结合使用。

因为结构是隐式密封的，所以无法继承它们。

有关详细信息，请参阅[继承](../../programming-guide/classes-and-structs/inheritance.md)。

有关更多示例，请参阅[抽象类、密封类及类成员](../../programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)。

## <a name="example"></a>示例

[!code-csharp[csrefKeywordsModifiers#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#17)]

在上面的示例中，可能会尝试使用以下语句从密封类继承：

`class MyDerivedC: SealedClass {}   // Error`

结果是出现错误消息：

`'MyDerivedC': cannot derive from sealed type 'SealedClass'`

## <a name="remarks"></a>备注

若要确定是否密封类、方法或属性，通常应考虑以下两点：

- 派生类通过可以自定义类而可能获得的潜在好处。

- 派生类可能采用使它们无法再正常工作或按预期工作的方式来修改类的可能性。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [静态类和静态类成员](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
- [抽象类、密封类及类成员](../../programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)
- [访问修饰符](../../programming-guide/classes-and-structs/access-modifiers.md)
- [修饰符](index.md)
- [override](override.md)
- [virtual](virtual.md)
