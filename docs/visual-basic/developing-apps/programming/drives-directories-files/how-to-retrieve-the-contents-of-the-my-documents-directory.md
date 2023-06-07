---
title: 如何：检索“我的文档”目录中的内容
ms.date: 07/20/2015
helpviewer_keywords:
- My Documents directory
ms.assetid: 26560d01-7dda-4457-8e95-21db23d71aea
ms.openlocfilehash: cf4470020507c581999b9d72602ddb6e3e76ed74
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334538"
---
# <a name="how-to-retrieve-the-contents-of-the-my-documents-directory-in-visual-basic"></a>如何：在 Visual Basic 中检索“我的文档”目录中的内容

可以使用 <xref:Microsoft.VisualBasic.FileIO.SpecialDirectories> 对象读取许多“所有用户”  目录，例如“我的文档”  或“桌面”  。  
  
### <a name="to-read-from-the-my-documents-folder"></a>读取“我的文档”文件夹  
  
- 可以使用 `ReadAllText`方法从特定目录中的每个文件中读取文本。 以下代码指定一个目录和文件，然后使用 `ReadAllText` 将它们读入名为 `patients` 的字符串。  
  
     [!code-vb[VbVbcnMyFileSystem#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.SpecialDirectories>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllText%2A>
