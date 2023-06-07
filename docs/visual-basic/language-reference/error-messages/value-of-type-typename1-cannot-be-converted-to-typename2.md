---
title: 类型“<typename1>”的值无法转换为“<typename2>”
ms.date: 07/20/2015
f1_keywords:
- vbc30955
- bc30955
helpviewer_keywords:
- BC30955
ms.assetid: 966b61eb-441e-48b0-bedf-ca95384ecb8b
ms.openlocfilehash: a4aab83fd5695633a660eab00a5032ffbe45f326
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161317"
---
# <a name="bc30955-value-of-type-typename1-cannot-be-converted-to-typename2"></a>BC30955：无法将类型 " \<typename1> " 的值转换为 " \<typename2> "

类型 "" 的值 \<typename1> 无法转换为 " \<typename2> "。 类型不匹配可能是由于将文件引用与程序集 "" 的项目引用混合而造成的 \<assemblyname> 。 尝试将对项目 "" 中 "" 的文件引用替换为 \<filepath> \<projectname1> 对 "" 的项目引用 \<projectname2> 。

 在项目同时进行项目引用和文件引用的情况下，编译器无法保证可以将一种类型转换为另一种类型。

 下面的伪代码说明了可能生成此错误的情况。

 `' ================ Visual Basic project P1 ================`

 `'        P1 makes a PROJECT REFERENCE to project P2`

 `'        and a FILE REFERENCE to project P3.`

 `Public commonObject As P3.commonClass`

 `commonObject = P2.getCommonClass()`

 `' ================ Visual Basic project P2 ================`

 `'        P2 makes a PROJECT REFERENCE to project P3`

 `Public Function getCommonClass() As P3.commonClass`

 `Return New P3.commonClass`

 `End Function`

 `' ================ Visual Basic project P3 ================`

 `Public Class commonClass`

 `End Class`

 项目 `P1` 通过项目进行间接项目引用 `P2` `P3` ，并对进行直接文件引用 `P3` 。 的声明 `commonObject` 使用对的文件引用 `P3` ，而对的调用 `P2.getCommonClass` 使用对的项目引用 `P3` 。

 出现这种情况的问题是，文件引用指定了 (的输出文件的文件路径和名称 `P3` （通常 p3.dll) ），而项目引用则 `P3` 按项目名称 () 标识源项目。 因此，编译器无法 `P3.commonClass` 通过两个不同的引用来保证该类型来自相同的源代码。

 当项目引用和文件引用混合时，通常会发生这种情况。 在上图中，如果 `P1` 直接引用项目 `P3` 而不是文件引用，则不会出现此问题。

 **错误 ID：** BC30955

## <a name="to-correct-this-error"></a>更正此错误

- 更改对项目引用的文件引用。

## <a name="see-also"></a>另请参阅

- [Visual Basic 中的类型转换](../../programming-guide/language-features/data-types/type-conversions.md)
- [管理项目中的引用](/visualstudio/ide/managing-references-in-a-project)
