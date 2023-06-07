---
title: 文件已打开
ms.date: 07/20/2015
f1_keywords:
- vbrID55
ms.assetid: d674a0fb-ef16-4cc2-9da7-709a8a07dbea
ms.openlocfilehash: ce8f8bf96d7355e45b2df82e8a131472c2ed2367
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162747"
---
# <a name="file-already-open"></a>文件已打开

有时，必须先关闭文件，然后才能 `FileOpen` 进行其他或其他操作。 此错误的可能原因包括：

- `FileOpen`为已打开的文件执行了顺序输出模式操作

- 语句引用打开的文件。

## <a name="to-correct-this-error"></a>更正此错误

- 在执行语句前关闭文件。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileSystem.FileOpen%2A>
