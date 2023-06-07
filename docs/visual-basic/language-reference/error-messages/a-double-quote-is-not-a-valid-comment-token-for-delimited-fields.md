---
title: 如果将 EscapeQuote 设置为 True，则双引号不是分隔字段的有效注释标记
ms.date: 07/20/2015
f1_keywords:
- vbrTextFieldParser_InvalidComment
ms.assetid: 636d4b81-00ba-4cfd-98f7-4d57036f494d
ms.openlocfilehash: a66d43d249a12ffa552073866f2e0a1e6d453608
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409930"
---
# <a name="a-double-quote-is-not-a-valid-comment-token-for-delimited-fields-where-escapequote-is-set-to-true"></a>如果将 EscapeQuote 设置为 True，则双引号不是分隔字段的有效注释标记

已提供了一个引号作为 `TextFieldParser`的分隔符，但 `EscapeQuotes` 已设置为 `True`。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 将 `EscapeQuotes` 设置为 `False`。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetDelimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.Delimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser>
- [如何：读取逗号分隔的文本文件](../../developing-apps/programming/drives-directories-files/how-to-read-from-comma-delimited-text-files.md)
