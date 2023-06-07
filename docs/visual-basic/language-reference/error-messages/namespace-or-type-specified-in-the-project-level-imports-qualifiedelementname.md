---
title: 在项目级 Imports“<qualifiedelementname>”中指定的命名空间或类型不包含任何公共成员，或者找不到该命名空间或类型
ms.date: 07/20/2015
f1_keywords:
- vbc40057
- bc40057
helpviewer_keywords:
- BC40057
ms.assetid: 4ae3506e-2ebe-4ff3-995d-14ac60db5e9f
ms.openlocfilehash: 54ee046cda998be8bd70e531918d6ab2a67d0494
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160114"
---
# <a name="bc40057-namespace-or-type-specified-in-the-project-level-imports-qualifiedelementname-doesnt-contain-any-public-member-or-cannot-be-found"></a>BC40057：在项目级 Imports "" 中指定的命名空间或类型 \<qualifiedelementname> 不包含任何公共成员，或者找不到该命名空间或类型

项目级 Imports "" 中指定的命名空间或类型 \<qualifiedelementname> 不包含任何公共成员，或者找不到该命名空间或类型。 请确保定义命名空间或类型，并且至少包含一个公共成员。 请确保别名不包含其他别名。

 项目的导入属性指定的包含元素无法找到或未定义任何 `Public` 成员。

 *包含元素*可以是命名空间、类、结构、模块、接口或枚举。 包含元素包含变量、过程或其他包含元素等成员。

 导入的目的是允许你的代码访问命名空间或类型成员，而无需对其进行限定。 你的项目可能还需要添加对命名空间或类型的引用。 有关详细信息，请参阅对已 [声明元素的引用](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)中的 "导入包含元素"。

 如果编译器找不到指定的包含元素，则它无法解析使用它的引用。 如果找到元素但元素未公开任何 `Public` 成员，则不会成功进行引用。 在这两种情况下，导入元素是毫无意义的。

 使用 " **项目设计器** " 可以指定要导入的元素。 使用 "**引用**" 页的 "**导入的命名空间**" 部分。 可以通过双击 "**解决方案资源管理器**中的 **" 我的项目**"图标来转到"**项目设计器**"。

 **错误 ID：** BC40057

## <a name="to-correct-this-error"></a>更正此错误

1. 打开 " **项目设计器** "，并切换到 " **引用** " 页。

2. 在 " **导入的命名空间** " 部分中，验证是否可以从项目访问包含元素。

3. 验证包含元素是否至少公开一个 `Public` 成员。

## <a name="see-also"></a>另请参阅

- [项目设计器 -&gt;“引用”页 (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
- [公共](../modifiers/public.md)
- [Visual Basic 中的命名空间](../../programming-guide/program-structure/namespaces.md)
- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
