---
title: 集合初始值设定项
ms.date: 07/20/2015
f1_keywords:
- vb.CollectionInitializer
helpviewer_keywords:
- collection initializers [Visual Basic]
ms.assetid: a9290329-77b0-4fdf-ae75-8fc17287f469
ms.openlocfilehash: 1d2d5adc7266faaa1636e568d6433429761eeaab
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414539"
---
# <a name="collection-initializers-visual-basic"></a>集合初始值设定项 (Visual Basic)

集合初始值设定项** 提供了用于创建集合并在其中填充一组初始值的缩短语法。 若要通过一组已知值（例如，菜单选项或类别列表、一组初始数字值、字符串（如日期或月份名称）或地理位置（如用于验证的州或省/自治区/直辖市列表）静态列表）创建集合，将会发现集合初始值设定项非常有用。

有关集合的详细信息，请参阅[集合](../../concepts/collections.md)。

在 `From` 关键字后面使用大括号 (`{}`) 来标识集合初始值设定项。 这类似于[数组](../arrays/index.md)中所述的数组文本语法。 下面的示例展示了使用集合初始值设定项创建集合的各种方法。

[!code-vb[VbVbalrCollectionInitializers#1](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#1)]

> [!NOTE]
> C# 也提供集合初始值设定项。 C# 集合初始值设定项的功能与 Visual Basic 集合初始值设定项相同。 有关 C# 集合初始值设定项的详细信息，请参阅[对象和集合初始值设定项](../../../../csharp/programming-guide/classes-and-structs/object-and-collection-initializers.md)。

## <a name="syntax"></a>语法

集合初始值设定项先是包含 `From` 关键字，后跟用大括号 (`{}`) 括住的逗号分隔值列表，如以下代码所示。

[!code-vb[VbVbalrCollectionInitializers#2](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#2)]

创建集合（如 <xref:System.Collections.Generic.List%601> 或 <xref:System.Collections.Generic.Dictionary%602>）时，必须在集合初始值设定项之前提供集合类型，如以下代码所示。

[!code-vb[VbVbalrCollectionInitializers#13](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#13)]

> [!NOTE]
> 不能合并集合初始值设定项和对象初始值设定项来初始化同一集合对象。 可以使用对象初始值设定项来初始化集合初始值设定项中的对象。

## <a name="creating-a-collection-by-using-a-collection-initializer"></a>使用集合初始值设定项创建集合

使用集合初始值设定项创建集合时，集合初始值设定项中提供的每个值都会传递到集合的相应 `Add` 方法中。 例如，如果使用集合初始值设定项创建 <xref:System.Collections.Generic.List%601>，那么集合初始值设定项中的每个字符串值都会传递到 <xref:System.Collections.Generic.List%601.Add%2A> 方法中。 若要使用集合初始值设定项创建集合，指定的类型必须是有效的集合类型。 有效的集合类型示例包括实现 <xref:System.Collections.Generic.IEnumerable%601> 接口或继承 <xref:System.Collections.CollectionBase> 类的类。 指定的类型还必须公开满足以下条件的 `Add` 方法。

- `Add` 方法必须可通过其中调用了集合初始值设定项的作用域获取。 若要在可以访问集合内非公开方法的方案中使用集合初始值设定项，不一定要公开 `Add` 方法。

- `Add` 方法必须是集合类的实例成员或 `Shared` 成员，或必须是扩展方法。

- `Add` 方法必须存在，可以根据重载解析规则与集合初始值设定项中提供的类型进行匹配。

 例如，下面的代码示例展示了如何使用集合初始值设定项创建 `List(Of Customer)` 集合。 代码运行时，每个 `Customer` 对象都会传递到泛型列表的 `Add(Customer)` 方法。

[!code-vb[VbVbalrCollectionInitializers#9](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#9)]

下面代码示例展示了不使用集合初始值设定项的等效代码。

[!code-vb[VbVbalrCollectionInitializers#10](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#10)]

如果集合内的 `Add` 方法包含与 `Customer` 对象的构造函数相匹配的参数，可以在集合初始值设定项内嵌套 `Add` 方法的参数值，如下一部分所述。 如果集合没有此类 `Add` 方法，可以创建一个作为扩展方法。 有关如何创建 `Add` 方法作为集合扩展方法的示例，请参阅[如何：创建集合初始值设定项使用的 Add 扩展方法](how-to-create-an-add-extension-method-used-by-a-collection-initializer.md)。 有关如何创建可用于集合初始值设定项的自定义集合的示例，请参阅[如何：创建集合初始值设定项使用的集合](how-to-create-a-collection-used-by-a-collection-initializer.md)。

## <a name="nesting-collection-initializers"></a>嵌套集合初始值设定项

可以在集合初始值设定项中嵌套值，从而标识要创建的集合的 `Add` 方法重载。 传递给 `Add` 方法的值必须用逗号隔开，并用大括号 (`{}`) 括住，就像在数组文本或集合初始值设定项中一样。

使用嵌套值创建集合时，嵌套值的每个元素都会作为参数传递到与元素类型匹配的 `Add` 方法中。 例如，下面的代码示例创建 <xref:System.Collections.Generic.Dictionary%602>，其中键类型为 `Integer`，值类型为 `String`。 每个嵌套值列表与 `Dictionary` 的 <xref:System.Collections.Generic.Dictionary%602.Add%2A> 方法匹配。

[!code-vb[VbVbalrCollectionInitializers#5](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#5)]

上面的代码示例等效于下面的代码。

[!code-vb[VbVbalrCollectionInitializers#6](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializers/VB/Module1.vb#6)]

只有来自第一级嵌套的嵌套值列表会发送给集合类型的 `Add` 方法。 深层次的嵌套会被视为数组文本，并且嵌套值列表不与任何集合的 `Add` 方法匹配。

## <a name="related-topics"></a>相关主题

|Title|说明|
|---|---|
|[如何：创建供集合初始化程序使用的 Add 扩展方法](how-to-create-an-add-extension-method-used-by-a-collection-initializer.md)|展示了如何创建 `Add` 扩展方法，以便在集合中填充集合初始值设定项中的值。|
|[如何：创建供集合初始化程序使用的集合](how-to-create-a-collection-used-by-a-collection-initializer.md)|展示了如何在实现 `IEnumerable` 的集合类中添加 `Add` 方法，从而启用集合初始值设定项。|

## <a name="see-also"></a>另请参阅

- [集合](../../concepts/collections.md)
- [数组](../arrays/index.md)
- [对象初始值设定项：命名和匿名类型](../objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [New 运算符](../../../language-reference/operators/new-operator.md)
- [自动实现的属性](../procedures/auto-implemented-properties.md)
- [如何：在 Visual Basic 中初始化数组变量](../arrays/how-to-initialize-an-array-variable.md)
- [局部类型推理](../variables/local-type-inference.md)
- [匿名类型](../objects-and-classes/anonymous-types.md)
- [Visual Basic 中的 LINQ 简介](../linq/introduction-to-linq.md)
- [如何：创建项列表](../../concepts/linq/how-to-create-a-list-of-items.md)
