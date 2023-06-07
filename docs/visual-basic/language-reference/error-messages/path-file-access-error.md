---
title: 路径/文件访问错误
ms.date: 07/20/2015
f1_keywords:
- vbrID75
ms.assetid: 6ce3a161-7316-46bd-a785-0d50e5414020
ms.openlocfilehash: 70de8f9cb33ab3d889f4916ae3d5de48cd218092
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871190"
---
# <a name="pathfile-access-error"></a>路径/文件访问错误

在文件访问或磁盘访问操作期间，操作系统无法在路径和文件名之间建立连接。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 请确保文件规范格式正确。 文件名可以包含完全限定的 (绝对) 或相对路径。 完全限定路径以驱动器名称开头 (如果路径位于另一个驱动器上) 并列出从根到文件的显式路径。 未完全限定的任何路径都是相对于当前驱动器和目录的路径。  
  
2. 请确保未尝试保存将替换现有只读文件的文件。 如果是这种情况，请更改目标文件的只读属性，或使用不同的文件名保存该文件。  
  
3. 请确保未尝试按顺序或模式打开只读文件 `Output` `Append` 。 如果是这种情况，请在模式下打开该文件 `Input` 或更改该文件的只读属性。  
  
4. 请确保未尝试在数据库或文档中更改 Visual Basic 项目。  
  
## <a name="see-also"></a>另请参阅

- [错误类型](../../programming-guide/language-features/error-types.md)
