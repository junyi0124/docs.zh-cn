---
title: “<expression>”不能用作类型约束
ms.date: 07/20/2015
f1_keywords:
- bc32061
- vbc32061
helpviewer_keywords:
- BC32061
ms.assetid: b17821b7-fa14-4397-a211-6e2a14079f09
ms.openlocfilehash: 6e55bfdc175f285b335512e64f4c2407bdb0e8c7
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163085"
---
# <a name="bc32061-expression-cannot-be-used-as-a-type-constraint"></a>BC32061： " \<expression> " 不能用作类型约束

约束列表包含的表达式不表示对类型形参的有效约束。

 约束列表对传递给类型形参的类型实参有一定要求。 你可以采用任意组合指定以下要求：

- 该类型实参必须实现一个或多个接口

- 该类型实参最多从一个类继承

- 类型实参必须公开一个创建代码可以访问的无形参构造函数（包括 `New` 约束）

 如果不在约束列表中包括任何特定类或接口，则可以通过指定以下内容之一来施加更常规的要求：

- 类型参数必须是值类型（包括 `Structure` 约束）

- 类型参数必须是引用类型（包括 `Class` 约束）

 不能为同一类型参数同时指定 `Structure` 和 `Class` ，并且它们两个都只能指定一次。

 **错误 ID：** BC32061

## <a name="to-correct-this-error"></a>更正此错误

- 验证表达式及其元素是否拼写正确。

- 如果表达式不符合前面列出的要求，请从约束列表中将其删除。

- 如果表达式引用接口或类，请验证编译器是否有权访问该接口或类。 可能需要限定其名称，并可能需要添加一个项目引用。 有关详细信息，请参阅对已 [声明元素的引用](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)中的 "项目引用"。

## <a name="see-also"></a>另请参阅

- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Value Types and Reference Types](../../programming-guide/language-features/data-types/value-types-and-reference-types.md)
- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
