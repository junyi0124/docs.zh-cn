---
title: Declared Element Names
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], case sensitivity
- names [Visual Basic], Visual Basic rules
- naming conventions [Visual Basic]
- element names [Visual Basic]
- declared elements [Visual Basic], identifiers
- declarations [Visual Basic], elements
- declared elements [Visual Basic], valid names
- '[] escape characters [Visual Basic]'
- names [Visual Basic], elements
- declaration statements [Visual Basic], declared elements
- declaring elements [Visual Basic]
- identifiers [Visual Basic], declared elements
- case sensitivity, declared element names
- escape characters [Visual Basic]
- names [Visual Basic], declared elements
- declared elements [Visual Basic], about declared elements
- escaped names [Visual Basic]
- declared elements [Visual Basic], names
- names [Visual Basic], naming conventions
- identifiers [Visual Basic], elements
ms.assetid: 09d8843b-c0dc-4afe-9dab-87c439a69e66
ms.openlocfilehash: 327a18644c1dc1d8dae59016b8e30600357d2ca9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086174"
---
# <a name="declared-element-names-visual-basic"></a>已声明的元素名称 (Visual Basic)

每个已声明的元素都有一个名称，也称为 *标识符*，代码使用该名称来引用它。  
  
## <a name="rules"></a>规则  

 Visual Basic 中的元素名称必须遵循以下规则：  
  
- 它必须以字母字符或下划线开头 (`_`) 。  
  
- 它只能包含字母字符、十进制数字和下划线。  
  
- 它必须至少包含一个字母字符或十进制数（如果以下划线开头）。  
  
- 长度不得超过1023个字符。  
  
 1023字符的长度限制还适用于完全限定名称（如）的整个字符串 `outerNamespace.middleNamespace.innerNamespace.thisClass.thisElement` 。  
  
 下面的示例显示了一些有效的元素名称。  
  
 `aB123__45`  
  
 `_567`  
  
 下面的示例显示一些无效的元素名称。 第一个仅包含下划线，第二个以十进制数字开头，第三个包含无效字符 ($) 。  
  
 `' Three INVALID element names`  
  
 `_`  
  
 `12ABC`  
  
 `xyz$wv`  
  
> [!CAUTION]
> 以下划线 () 开头的元素名称 `_` 不属于 [语言独立性和与语言无关的组件](../../../../standard/language-independence-and-language-independent-components.md) (CLS) ，因此符合 cls 的代码不能使用定义此类名称的组件。 但元素名称中任何其他位置的下划线都符合 CLS。  
  
### <a name="name-length-guidelines"></a>名称长度准则  

 在实际情况下，你的名称应尽可能简短，同时仍能清楚地确定元素的性质。 这可以提高代码的可读性，并减少行长度和源文件大小。  
  
 另一方面，你的名称不应太短，因为它没有充分描述元素所表示的内容以及你的代码如何使用它。 这对于代码的可读性非常重要。 如果其他人正在尝试理解它，或者你在写入它后你需要很长的时间，则适当的元素名称可以节省相当多的时间。  
  
## <a name="escaped-names"></a>转义名称  

 通常，元素名称不能与 Visual Basic 保留的任何关键字（如或）匹配 `Case` `Friend` 。 但是，您可以定义一个 *转义名称*，该名称由方括号 () 括起来 `[ ]` 。 转义名称可以匹配任何 Visual Basic 关键字，因为方括号会删除任何不明确的名称。 稍后在代码中引用名称时也会用到方括号。  
  
 通常，仅当以下情况下，才应使用转义名称：  
  
- 你的代码已从以前版本的 Visual Basic 迁移，此版本未保留用作名称的关键字;或  
  
- 你使用的是以另一种语言编写的代码，但给定的关键字不是保留关键字。  
  
 否则，如果元素的名称与关键字冲突，则应考虑重命名该元素。 集成开发环境 (IDE) 提供一种简单的方法来实现此目的。 有关详细信息，请参阅 [重构](/visualstudio/ide/refactoring-in-visual-studio)。  
  
## <a name="case-sensitivity-in-names"></a>名称区分大小写  

 Visual Basic 中的元素名称不区分大小写。 这意味着当编译器仅对字母大小写不同的两个名称进行比较时，它会将它们解释为同一名称。 例如，它认为 `ABC` 和 `abc` 指的是同一个声明的元素。  
  
 但是，公共语言运行时 (CLR) 使用区分大小写绑定。 因此，当你生成程序集或 DLL 并使其可供其他程序集使用时，则名称将不再不区分大小写。 例如，如果你用名为 `ABC`的元素定义类，而其他程序集通过公共语言运行时使用你的类，则元素必须指的是 `ABC`。 如果随后重新编译类并将元素名称更改为 `abc` ，则使用类的其他程序集将无法再访问该元素。 因此，发布程序集的更新版本时，不应该更改公共元素的字母大小写。  
  
## <a name="names-and-locales"></a>名称和区域设置  

 名称比较与区域设置无关。 如果两个名称在一个区域设置中匹配，则保证它们在所有区域设置中匹配。  
  
## <a name="see-also"></a>请参阅

- [已声明元素](index.md)
- [已声明元素的特性](declared-element-characteristics.md)
- [References to Declared Elements](references-to-declared-elements.md)
- [语句](../../../language-reference/statements/index.md)
