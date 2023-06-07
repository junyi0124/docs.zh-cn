---
title: 如何：创建新变量
ms.date: 07/20/2015
helpviewer_keywords:
- Dim statement [Visual Basic]
- variables [Visual Basic], creating
ms.assetid: 35300be3-77b0-4bef-a156-034d3cdedde0
ms.openlocfilehash: 501c8f3aff4c5b204d231bdc26213e13d125a7cf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410524"
---
# <a name="how-to-create-a-new-variable-visual-basic"></a>如何：创建新变量 (Visual Basic)

使用[Dim 语句](../../../language-reference/statements/dim-statement.md)创建变量。

### <a name="to-create-a-new-variable"></a>创建新变量

1. 在语句中声明变量 `Dim` 。

    ```vb
    Dim newCustomer
    ```

2. 包含变量特征的规范，如[Private](../../../language-reference/modifiers/private.md)、 [Static](../../../language-reference/modifiers/static.md)、 [Shadows](../../../language-reference/modifiers/shadows.md)或[WithEvents](../../../language-reference/modifiers/withevents.md)。 有关详细信息，请参阅[声明的元素特征](../declared-elements/declared-element-characteristics.md)。

    ```vb
    Public Static newCustomer
    ```

    `Dim`如果在声明中使用其他关键字，则不需要使用关键字。

3. 请按照该规范的名称进行操作，该变量的名称必须遵循 Visual Basic 规则和约定。 有关详细信息，请参阅已[声明的元素名称](../declared-elements/declared-element-names.md)。

    ```vb
    Public Static newCustomer
    ```

4. 在名称后跟[As](../../../language-reference/statements/as-clause.md)子句以指定变量的数据类型。

    ```vb
    Public Static newCustomer As Customer
    ```

    如果未指定数据类型，则将使用默认值： `Object` 。

5. `As`使用等号（）跟随子句 `=` ，并在该变量的初始值后跟等号。

    每次运行语句时，Visual Basic 都将指定的值分配给变量 `Dim` 。 如果不指定初始值，则 Visual Basic 在首次输入包含该语句的代码时，为该变量的数据类型分配默认初始值 `Dim` 。

    如果变量是引用类型，则可以通过在子句中包含[New 运算符](../../../language-reference/operators/new-operator.md)关键字来创建其类的实例 `As` 。 如果不使用，则 `New` 变量的初始值为[Nothing](../../../language-reference/nothing.md)。

    ```vb
    Public Static newCustomer As New Customer
    ```

## <a name="see-also"></a>另请参阅

- [变量](index.md)
- [变量声明](variable-declaration.md)
- [Declared Element Names](../declared-elements/declared-element-names.md)
- [已声明元素的特性](../declared-elements/declared-element-characteristics.md)
- [Value Types and Reference Types](../data-types/value-types-and-reference-types.md)
- [语句](../../../language-reference/statements/index.md)
- [局部类型推理](local-type-inference.md)
- [Option Infer 语句](../../../language-reference/statements/option-infer-statement.md)
