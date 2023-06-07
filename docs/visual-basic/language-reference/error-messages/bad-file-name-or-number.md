---
title: 错误的文件名或文件号
ms.date: 07/20/2015
f1_keywords:
- vbrID52
ms.assetid: d0e96aea-7621-48f6-a78b-5d37d18aaa4e
ms.openlocfilehash: d57a9e78e6ae179d3050e5a92399ca731fa16ba7
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874907"
---
# <a name="bad-file-name-or-number"></a>错误的文件名或文件号

尝试访问指定的文件时出错。 此错误的可能原因包括：  
  
- 语句引用的文件具有未在语句中指定 `FileOpen` 或在语句中指定的、 `FileOpen` 但随后已关闭的文件。  
  
- 语句引用的文件的数量超出了文件编号范围。  
  
- 语句引用的文件名或编号无效。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 请确保在语句中指定了文件名 `FileOpen` 。 请注意，如果调用 `FileClose` 不带参数的语句，则可能会无意中关闭所有打开的文件。  
  
2. 如果你的代码生成了文件号算法，请确保数字有效。  
  
3. 检查文件名，确保它们符合操作系统约定。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileSystem.FileOpen%2A>
- [Visual Basic 命名约定](../../programming-guide/program-structure/naming-conventions.md)
