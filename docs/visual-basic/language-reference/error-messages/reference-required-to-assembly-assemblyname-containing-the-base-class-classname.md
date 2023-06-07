---
title: 需要对程序集“<assemblyname>”（包含基类“<classname>”）的引用
ms.date: 07/20/2015
f1_keywords:
- bc30007
- vbc30007
helpviewer_keywords:
- BC30007
ms.assetid: 5f34cf47-6c6e-4954-bd8e-d6b020b75fb7
ms.openlocfilehash: d2fb3498219dfe3318ec418ede250de818874ba9
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162331"
---
# <a name="bc30007-reference-required-to-assembly-assemblyname-containing-the-base-class-classname"></a>BC30007：需要对 \<assemblyname> 包含基类 "" 的程序集 "" 的引用 \<classname>

需要对 \<assemblyname> 包含基类 "" 的程序集 "" 的引用 \<classname> 。 请向项目中添加一个。

 该类是在动态链接库 (DLL) 或未在项目中直接引用的程序集中定义的。 如果类是在多个 DLL 或程序集中定义的，则 Visual Basic 编译器需要引用以避免多义性。

 **错误 ID：** BC30007

## <a name="to-correct-this-error"></a>更正此错误

- 将未引用的 DLL 或程序集名称包括在项目引用中。

## <a name="see-also"></a>另请参阅

- [管理项目中的引用](/visualstudio/ide/managing-references-in-a-project)
- [Troubleshooting Broken References](/visualstudio/ide/troubleshooting-broken-references)
