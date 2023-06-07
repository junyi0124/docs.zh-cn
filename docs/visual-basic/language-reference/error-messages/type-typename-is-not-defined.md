---
title: 未定义类型“<typename>”
ms.date: 07/20/2015
f1_keywords:
- vbc30002
- bc30002
helpviewer_keywords:
- BC30002
ms.assetid: b0faf204-57fd-44de-8c05-9db027eea663
ms.openlocfilehash: 195e749e29494d438dbd052e8e308250f4cce1ca
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161889"
---
# <a name="bc30002-type-typename-is-not-defined"></a>BC30002： \<typename> 未定义类型 ""

语句引用了未定义的类型。 可在声明语句（如、、或）中定义类型 `Enum` `Structure` `Class` `Interface` 。

 **错误 ID：** BC30002

## <a name="to-correct-this-error"></a>更正此错误

- 确保类型定义及其引用均使用相同的拼写。

- 确保引用可以访问类型定义。 例如，如果类型在另一个模块中并且已被声明 `Private` ，请将类型定义移到引用模块，或将其声明为 `Public` 。

- 确保未在项目中重新定义类型的命名空间。 如果是，则使用 `Global` 关键字完全限定类型名称。 例如，如果项目定义名为的命名空间 `System` ，则 <xref:System.Object?displayProperty=nameWithType> 无法访问该类型，除非它是用关键字：完全限定的 `Global` `Global.System.Object` 。

- 如果定义了该类型，但在 Visual Basic 中未注册定义该类型的对象库或类型库，请单击 "**项目**" 菜单上的 "**添加引用**"，然后选择相应的对象库或类型库。

- 确保该类型位于作为目标 .NET Framework 配置文件的一部分的程序集中。 有关详细信息，请参阅 [.NET Framework 目标错误疑难解答](/visualstudio/msbuild/troubleshooting-dotnet-framework-targeting-errors)。

## <a name="see-also"></a>另请参阅

- [Visual Basic 中的命名空间](../../programming-guide/program-structure/namespaces.md)
- [Enum 语句](../statements/enum-statement.md)
- [Structure 语句](../statements/structure-statement.md)
- [Class 语句](../statements/class-statement.md)
- [Interface 语句](../statements/interface-statement.md)
- [管理项目中的引用](/visualstudio/ide/managing-references-in-a-project)
