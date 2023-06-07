---
title: 根命名空间 <namespacename> 中的名称 <fullnamespacename> 不符合 CLS
ms.date: 07/20/2015
f1_keywords:
- vbc40039
- bc40039
helpviewer_keywords:
- BC40039
ms.assetid: c5bd5914-ae71-416a-8bed-f76f644f78be
ms.openlocfilehash: 48e020cb74641279a4f402e47034514d78dfc4fc
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160205"
---
# <a name="bc40039-name-namespacename-in-the-root-namespace-fullnamespacename-is-not-cls-compliant"></a>BC40039： \<namespacename> 根命名空间中的名称 \<fullnamespacename> 不符合 CLS

程序集标记为 `<CLSCompliant(True)>` ，但根命名空间名称的元素以下划线 (`_`) 开头。

 编程元素可以包含一个或多个下划线，但要符合 [语言独立性和 Language-Independent 组件](../../../standard/language-independence-and-language-independent-components.md) (CLS) ，它不能以下划线开头。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。

 当将 <xref:System.CLSCompliantAttribute> 应用到编程元素中时，需要将该特性的 `isCompliant` 参数设置为 `True` 或 `False` 来指示符合或不符合性。 此参数没有默认值，必须为其提供一个值。

 如果不将 <xref:System.CLSCompliantAttribute> 应用到元素，则它将被视为不符合规范。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC40039

## <a name="to-correct-this-error"></a>更正此错误

- 如果需要符合 CLS，请更改根命名空间名称，使其所有元素都不以下划线开头。

- 如果要求命名空间名称保持不变，则 <xref:System.CLSCompliantAttribute> 从程序集删除或将其标记为 `<CLSCompliant(False)>` 。

## <a name="see-also"></a>另请参阅

- [Namespace 语句](../statements/namespace-statement.md)
- [Visual Basic 中的命名空间](../../programming-guide/program-structure/namespaces.md)
- [-rootnamespace](../../reference/command-line-compiler/rootnamespace.md)
- [“项目设计器”->“应用程序”页 (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
- [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [Visual Basic 命名约定](../../programming-guide/program-structure/naming-conventions.md)
