---
title: Visual Basic 程序的结构
ms.date: 07/20/2015
helpviewer_keywords:
- conditional compilation [Visual Basic], Visual Basic
- program structure [Visual Basic], Visual Basic
- procedures [Visual Basic], structure
- Visual Basic code, program structure
ms.assetid: ad0c6531-d762-4c77-a700-de16b07b6119
ms.openlocfilehash: 90bc1fd62a05f670424e1fac368376401d1030c0
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097769"
---
# <a name="structure-of-a-visual-basic-program"></a>Visual Basic 程序的结构

Visual Basic 程序由标准构建基块构建。 *解决方案*由一个或多个项目组成。 一个 *项目* 又可以包含一个或多个程序集。 每个 *程序集* 都从一个或多个源文件进行编译。 *源文件*提供类、结构、模块和接口的定义和实现，它们最终包含所有代码。  
  
 有关 Visual Basic 程序的这些构建基块的详细信息，请参阅 .NET 中的 [解决方案和项目](/visualstudio/ide/solutions-and-projects-in-visual-studio) 和 [程序集](../../../standard/assembly/index.md)。  
  
## <a name="file-level-programming-elements"></a>文件级编程元素  

 当您启动项目或文件并打开 "代码编辑器" 时，您将看到一些代码已存在并按正确的顺序排列。 你编写的任何代码都应遵循以下顺序：  
  
1. `Option` 前瞻性  
  
2. `Imports` 前瞻性  
  
3. `Namespace` 语句和命名空间级别元素  
  
 如果按不同的顺序输入语句，则可能会导致编译错误。  
  
 程序还可以包含条件编译语句。 可以在上述序列的语句之间点播这些文件。  
  
### <a name="option-statements"></a>Option 语句  

 `Option` 语句为后续代码建立基本规则，帮助防止语法和逻辑错误。 [Option Explicit 语句](../../language-reference/statements/option-explicit-statement.md)可确保所有变量都已正确声明和拼写，从而减少了调试时间。 [Option Strict 语句](../../language-reference/statements/option-strict-statement.md)有助于最大程度减少在不同数据类型的变量之间工作时可能发生的逻辑错误和数据丢失。 [Option Compare 语句](../../language-reference/statements/option-compare-statement.md)根据它们的或值来指定字符串的比较方式 `Binary` `Text` 。  
  
### <a name="imports-statements"></a>Imports 语句  

 可以在 [.Net 命名空间 ( 包含 Imports 语句，并键入) ](../../language-reference/statements/imports-statement-net-namespace-and-type.md) 导入项目外部定义的名称。 `Imports`通过语句，你的代码可以引用在导入的命名空间中定义的类和其他类型，而无需对它们进行限定。 您可以根据需要使用任意多个 `Imports` 语句。 有关详细信息，请参阅 [引用和 Imports 语句](references-and-the-imports-statement.md)。  
  
### <a name="namespace-statements"></a>Namespace 语句  

 命名空间可帮助你组织和分类编程元素以便于分组和访问。 使用 [Namespace 语句](../../language-reference/statements/namespace-statement.md) 将特定命名空间中的以下语句分类。 有关详细信息，请参阅 [Visual Basic 中的命名空间](namespaces.md)。  
  
### <a name="conditional-compilation-statements"></a>条件编译语句  

 条件编译语句几乎可以出现在源文件中的任何位置。 它们会在编译时包含或排除部分代码，具体取决于特定的条件。 你还可以使用它们来调试应用程序，因为条件代码仅在调试模式下运行。 有关详细信息，请参阅 [条件编译](conditional-compilation.md)。  
  
## <a name="namespace-level-programming-elements"></a>命名空间级编程元素  

 类、结构和模块包含源文件中的所有代码。 它们是 *命名空间级别* 的元素，可出现在命名空间或源文件级别。 它们保留所有其他编程元素的声明。 接口用于定义元素签名，但不提供实现，也显示在模块级别。 有关模块级元素的详细信息，请参阅以下内容：  
  
- [Class 语句](../../language-reference/statements/class-statement.md)  
  
- [Structure 语句](../../language-reference/statements/structure-statement.md)  
  
- [Module 语句](../../language-reference/statements/module-statement.md)  
  
- [Interface 语句](../../language-reference/statements/interface-statement.md)  
  
 命名空间级别的数据元素是枚举和委托。  
  
## <a name="module-level-programming-elements"></a>模块级编程元素  

 过程、运算符、属性和事件是唯一的编程元素，这些元素可以保存可执行代码 (在运行时) 执行操作的语句。 它们是程序的 *模块级* 元素。 有关过程级元素的详细信息，请参阅以下内容：  
  
- [Function 语句](../../language-reference/statements/function-statement.md)  
  
- [Sub 语句](../../language-reference/statements/sub-statement.md)  
  
- [Declare Statement](../../language-reference/statements/declare-statement.md)  
  
- [Operator Statement](../../language-reference/statements/operator-statement.md)  
  
- [Property Statement](../../language-reference/statements/property-statement.md)  
  
- [Event 语句](../../language-reference/statements/event-statement.md)  
  
 模块级的数据元素包括变量、常量、枚举和委托。  
  
## <a name="procedure-level-programming-elements"></a>过程级编程元素  

 *过程级*元素的大部分内容是可执行语句，这些语句构成程序的运行时代码。 所有可执行代码都必须在 (`Function` 、 `Sub` 、、 `Operator` `Get` 、、、) `Set` `AddHandler` `RemoveHandler` `RaiseEvent` 的过程中。 有关详细信息，请参阅[语句](../language-features/statements.md)。  
  
 过程级别的数据元素仅限于局部变量和常量。  
  
## <a name="the-main-procedure"></a>Main 过程  

 此 `Main` 过程是加载应用程序时要运行的第一个代码。 `Main` 用作应用程序的起点和总体控制。 有四种种类的 `Main` ：  
  
- `Sub Main()`  
  
- `Sub Main(ByVal cmdArgs() As String)`  
  
- `Function Main() As Integer`  
  
- `Function Main(ByVal cmdArgs() As String) As Integer`  
  
 此过程最常见的方法是 `Sub Main()` 。 有关详细信息，请参阅 [中的 Main 过程 Visual Basic](main-procedure.md)。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 中的 Main 过程](main-procedure.md)
- [Visual Basic 命名约定](naming-conventions.md)
- [Visual Basic 限制](limitations.md)
