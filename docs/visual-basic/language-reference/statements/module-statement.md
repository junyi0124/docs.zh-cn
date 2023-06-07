---
title: Module 语句
ms.date: 07/20/2015
f1_keywords:
- Module
- vb.Module
helpviewer_keywords:
- modules, classes
- modules
- Module statement [Visual Basic]
- modules, declaring
- standard modules
- classes [Visual Basic], vs. modules
- declarations [Visual Basic], modules
ms.assetid: a1243afc-14a5-45df-95d5-51118aeac362
ms.openlocfilehash: 24a27ba41f5ac889f2f2725a2852368a4292a6fb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404444"
---
# <a name="module-statement"></a>Module 语句

声明模块的名称，并引入模块包含的变量、属性、事件和过程的定义。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ accessmodifier ]  Module name
    [ statements ]
End Module
```

## <a name="parts"></a>组成部分

`attributelist`  
可选。 请参阅[特性列表](attribute-list.md)。

`accessmodifier`  
可选。 可以是以下其中一个值：

- [公共](../modifiers/public.md)

- [友好](../modifiers/friend.md)

请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

`name`  
必需。 此模块的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。

`statements`  
可选。 定义此模块的变量、属性、事件、过程和嵌套类型的语句。

`End Module`  
终止 `Module` 定义。

## <a name="remarks"></a>备注

`Module`语句定义在其命名空间中可用的引用类型。 *模块*（有时称为*标准模块*）与类相似，但有一些重要的区别。 每个模块都有一个实例，无需创建或分配给变量。 模块不支持继承或实现接口。 请注意，如果某个模块不是类或结构的*类型*，则不能将编程元素声明为具有模块的数据类型。

只能 `Module` 在命名空间级别使用。 这意味着模块的*声明上下文*必须是源文件或命名空间，不能是类、结构、模块、接口、过程或块。 不能将模块嵌套在其他模块或任何类型中。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

模块与程序具有相同的生存期。 因为其成员都是 `Shared` ，所以它们的生存期也等于程序的生存期。

模块默认为[Friend](../modifiers/friend.md)访问。 您可以使用访问修饰符调整其访问级别。 有关详细信息，请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

模块的所有成员都是隐式的 `Shared` 。

## <a name="classes-and-modules"></a>类和模块

这些元素具有许多相似之处，但也存在一些重要的差异。

- **规范.** Visual Basic 的早期版本可识别两种类型的模块：*类模块*（cls 文件）和*标准模块*（bas 文件）。 当前版本分别调用这些*类*和*模块*。

- **共享成员。** 您可以控制某个类的成员是共享成员还是实例成员。

- **对象方向。** 类是面向对象的，但模块不是。 因此，只有类可以实例化为对象。 有关详细信息，请参阅[对象和类](../../programming-guide/language-features/objects-and-classes/index.md)。

## <a name="rules"></a>规则

- **组成.** 所有模块成员都是隐式[共享](../modifiers/shared.md)的。 不能在 `Shared` 声明成员时使用关键字，也不能更改任何成员的共享状态。

- **继承.** 模块不能从 <xref:System.Object> 所有模块继承的类型继承。 特别是，一个模块不能从另一个模块继承。

  不能在模块定义中使用[Inherits 语句](inherits-statement.md)，甚至可以指定 <xref:System.Object> 。

- **默认属性。** 不能在模块中定义任何默认属性。 有关详细信息，请参阅[默认值](../modifiers/default.md)。

## <a name="behavior"></a>行为

- **访问级别。** 在模块中，你可以使用其自己的访问级别声明每个成员。 模块成员默认为[公共](../modifiers/public.md)访问权限，变量和常量除外，默认为[私有](../modifiers/private.md)访问。 当某个模块的访问权限超过其成员之一时，将优先使用指定的模块访问级别。

- **内.** 模块在其命名空间中的作用域。

  每个模块成员的作用域都是整个模块。 请注意，所有成员都接受*类型提升*，这会导致其作用域升级到包含该模块的命名空间。 有关详细信息，请参阅[类型提升](../../programming-guide/language-features/declared-elements/type-promotion.md)。

- **限定.** 项目中可以有多个模块，可以在两个或多个模块中声明同名的成员。 但是，如果引用来自模块外部，则必须使用适当的模块名称来限定对此类成员的任何引用。 有关详细信息，请参阅 [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)。

## <a name="example"></a>示例

[!code-vb[VbVbalrStatements#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#69)]

## <a name="see-also"></a>另请参阅

- [Class 语句](class-statement.md)
- [Namespace 语句](namespace-statement.md)
- [Structure 语句](structure-statement.md)
- [Interface 语句](interface-statement.md)
- [Property Statement](property-statement.md)
- [类型提升](../../programming-guide/language-features/declared-elements/type-promotion.md)
