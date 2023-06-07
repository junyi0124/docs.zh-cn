---
title: 编码不能设置为 Nothing。
ms.date: 07/20/2015
ms.assetid: 59f7c731-8291-4a85-bf51-c225e48cdc84
ms.openlocfilehash: 0356098ca3fb41804ea396b0ff792cf2990b3340
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077535"
---
# <a name="encoding-cannot-be-set-to-nothing"></a>编码不能设置为 Nothing。

尝试读取或写入文件失败，因为已将参数 `encoding` 设置为 `Nothing` ，但需要有效值。  
  
 <xref:System.Text.Encoding> 用于确定写入文件时使用何种编码。 默认为 UTF-8。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 为 `encoding` 参数提供有效值。  
  
## <a name="see-also"></a>请参阅

- [文件编码](../developing-apps/programming/drives-directories-files/file-encodings.md)
- [从文件读取](../developing-apps/programming/drives-directories-files/reading-from-files.md)
- [写入文件](../developing-apps/programming/drives-directories-files/writing-to-files.md)
- [文件 System.io.file.readalltext](xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllText%2A)
- [文件 Microsoft.visualbasic.fileio.filesystem.writealltext](xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A)
