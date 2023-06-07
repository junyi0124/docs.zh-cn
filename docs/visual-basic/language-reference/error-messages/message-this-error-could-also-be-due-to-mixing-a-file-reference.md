---
title: <message>此错误也可能是由于将程序集“<assemblyname>”的文件引用与项目引用混合使用所造成的
ms.date: 07/20/2015
f1_keywords:
- bc30971
- vbc30971
helpviewer_keywords:
- BC30971
ms.assetid: 75d2e8b5-2fdc-4623-8b32-cba805dab7db
ms.openlocfilehash: 9340c5c58c0cdb70c517534a339f57eb9ec1f906
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162422"
---
# <a name="bc30971-message-this-error-could-also-be-due-to-mixing-a-file-reference-with-a-project-reference-to-assembly-assemblyname"></a>BC30971： \<message> 此错误也可能是由于将文件引用与程序集 "" 的项目引用混合使用所造成的 \<assemblyname>

\<message> 此错误还可能是由于将文件引用与程序集 "的项目引用混合而造成的 \<assemblyname> 。 在这种情况下，尝试将项目 "" 中对 "" 的文件引用替换为 \<assemblyfilename> \<projectname1> 对 "" 的项目引用 \<projectname2> 。

 项目中的代码访问另一个项目的成员，但你的解决方案配置不允许 Visual Basic 编译器解析引用。

 若要访问另一个程序集中定义的类型，Visual Basic 编译器必须具有对该程序集的引用。 此引用必须单一、明确，不会导致项目之间循环引用。

 **错误 ID：** BC30971

## <a name="to-correct-this-error"></a>更正此错误

1. 确定产生最佳程序集引用的项目。 为便于确定，你可以使用文件访问轻松程度和更新频率等条件。

2. 在项目属性中，添加对包含某程序集的项目的引用，该程序集定义正在使用的类型。

## <a name="see-also"></a>另请参阅

- [管理项目中的引用](/visualstudio/ide/managing-references-in-a-project)
- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)

- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
- [Troubleshooting Broken References](/visualstudio/ide/troubleshooting-broken-references)
