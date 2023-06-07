---
title: 权限被拒绝
ms.date: 07/20/2015
f1_keywords:
- vbrID70
ms.assetid: 71f46756-f522-4814-aab4-492bf9924245
ms.openlocfilehash: 5ac585a86a783f36642545e368b1c2e694b89f62
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871163"
---
# <a name="permission-denied-visual-basic"></a>权限被拒绝 (Visual Basic)

尝试写入到写保护的磁盘或访问锁定的文件。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 若要打开写保护的文件，请更改文件的写保护特性。  
  
2. 请确保其他进程未锁定该文件，并等待打开该文件，直到另一个进程释放它。  
  
3. 若要访问注册表，请检查你的用户权限是否包括此类型的注册表访问权限。  
  
## <a name="see-also"></a>另请参阅

- [错误类型](../../programming-guide/language-features/error-types.md)
