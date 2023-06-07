---
title: 命名约定
ms.date: 07/20/2015
helpviewer_keywords:
- names [Visual Basic], Visual Basic rules
- naming conventions
- naming conventions [Visual Basic], Visual Basic
- Visual Basic code, naming conventions
- conventions [Visual Basic], Visual Basic coding
- names [Visual Basic], naming conventions
- naming conventions [Visual Basic], classes
ms.assetid: 164949a4-2a7c-4736-9d82-9c3078e2e56c
ms.openlocfilehash: b25d246bd31147b7a9ba2c72214926fdb5ca8895
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072140"
---
# <a name="visual-basic-naming-conventions"></a>Visual Basic 命名约定

在 Visual Basic 应用程序中为某个元素命名时，该名称的第一个字符必须是字母字符或下划线。 但请注意，以下划线开头的名称不符合 [语言独立性和与语言无关的组件](../../../standard/language-independence-and-language-independent-components.md) (CLS) 。  
  
 以下建议适用于命名。  
  
- 以大写字母开头每个单独的单词，如和中所示 `FindLastRecord` `RedrawMyForm` 。  
  
- 使用谓词作为函数和方法名称的开头，如或中所示 `InitNameArray` `CloseDialog` 。  
  
- 以名词开头的类、结构、模块和属性名称，如 `EmployeeName` 或 `CarAccessory` 。  
  
- 以前缀 "I" 开始接口名称，后跟名词或名词短语，如 `IComponent` ，或带有描述接口行为的形容词（如） `IPersistable` 。 不要使用下划线，并慎用缩写，因为缩写会导致混淆。  
  
- 用名词开始事件处理程序名称，该名称描述后跟 "" 后缀的事件类型 `EventHandler` ，如 " `MouseEventHandler` "。  
  
- 在事件参数类的名称中，包含 " `EventArgs` " 后缀。  
  
- 如果某个事件具有 "before" 或 "after" 这一概念，则使用现在时或过去时形式的后缀，如 " `ControlAdd` " 或 " `ControlAdded` "。  
  
- 对于长或频繁使用的术语，请使用缩写使名称长度合理，例如 "HTML"，而不是 "超文本标记语言"。 通常，超过32个字符的变量名称在设置为低分辨率的监视器上难以读取。 此外，请确保缩写在整个应用程序中保持一致。 在 "HTML" 和 "超文本标记语言" 之间随机切换项目可能会导致混淆。  
  
- 避免使用与外部作用域中的名称相同的内部作用域中的名称。 如果访问错误的变量，则会导致错误。 如果变量与同名的关键字之间发生冲突，则必须通过在其前面加上适当的类型库来标识关键字。 例如，如果你有一个名为的变量 `Date` ，则只能通过调用来使用内部 `Date` 函数 <xref:System.DateTime.Date%2A?displayProperty=nameWithType> 。  
  
## <a name="see-also"></a>请参阅

- [代码中用作元素名称的关键字](keywords-as-element-names-in-code.md)
- [Me、My、MyBase 和 MyClass](me-my-mybase-and-myclass.md)
- [Declared Element Names](../language-features/declared-elements/declared-element-names.md)
- [程序结构和代码约定](program-structure-and-code-conventions.md)
- [Visual Basic 语言参考](../../language-reference/index.md)
