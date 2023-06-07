---
title: 分隔符不能为 Nothing 或空字符串
ms.date: 07/20/2015
f1_keywords:
- vbrTextFieldParser_DelimiterNothing
ms.assetid: 8885fcd1-c201-409d-9a32-6ff2b13c0c13
ms.openlocfilehash: 786eb2bddc967e9b5b8fd6afec3ffbe264814834
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91084926"
---
# <a name="a-delimiter-cannot-be-nothing-or-an-empty-string"></a>分隔符不能为 Nothing 或空字符串

`TextFieldParser` 无法读取文件，因为 `Delimiters` 属性被设置为 `Nothing` 或为空 `String` ("")。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 提供 `Delimiters`的有效值。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetDelimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.Delimiters>
- [如何：读取逗号分隔的文本文件](../developing-apps/programming/drives-directories-files/how-to-read-from-comma-delimited-text-files.md)
- [TextFieldParser Object](../language-reference/objects/textfieldparser-object.md)
- [使用 TextFieldParser 对象分析文本文件](../developing-apps/programming/drives-directories-files/parsing-text-files-with-the-textfieldparser-object.md)
