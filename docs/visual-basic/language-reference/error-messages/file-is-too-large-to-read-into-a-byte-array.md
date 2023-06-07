---
title: 文件太大，无法读取到字节数组中
ms.date: 07/20/2015
ms.assetid: 686630a6-a439-46c7-8d7b-34613ae4c5d8
ms.openlocfilehash: 89459aaf766a70f69008f847dda7ac6e2a1e41d1
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874191"
---
# <a name="file-is-too-large-to-read-into-a-byte-array"></a>文件太大，无法读取到字节数组中

尝试读入字节数组的文件大小超过 4 GB。 `My.Computer.FileSystem.ReadAllBytes`此方法无法读取超过此大小的文件。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 使用 <xref:System.IO.StreamReader> 读取文件。 有关详细信息，请参阅 [Visual Basic) 中 .NET Framework 文件 i/o 和文件系统 (的基础知识 ](../../developing-apps/programming/drives-directories-files/basics-of-net-framework-file-io-and-the-file-system.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllBytes%2A>
- <xref:System.IO.StreamReader>
- [使用 Visual Basic 访问文件](../../developing-apps/programming/drives-directories-files/file-access.md)
- [如何：使用 StreamReader 读取文件中的文本](../../developing-apps/programming/drives-directories-files/how-to-read-text-from-files-with-a-streamreader.md)
