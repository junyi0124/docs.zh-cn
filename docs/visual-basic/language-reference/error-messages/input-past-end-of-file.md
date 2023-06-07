---
title: 输入超出文件尾
ms.date: 07/20/2015
f1_keywords:
- vbrID62
ms.assetid: 65292704-6e7d-4622-9f50-eb655a59b016
ms.openlocfilehash: c0cb0373fb0652e9600ac8651661708414561aca
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873970"
---
# <a name="input-past-end-of-file"></a>输入超出文件尾

`Input`语句读取的文件是空的，或者是使用所有数据的文件，或者使用的是以 `EOF` 二进制访问方式打开的文件。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 在 `EOF` 语句前直接使用函数 `Input` 来检测文件的结尾。  
  
2. 如果打开文件以进行二进制访问，请使用 `Seek` 和 `Loc` 。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileSystem.Input%2A>
- <xref:Microsoft.VisualBasic.FileSystem.EOF%2A>
- <xref:Microsoft.VisualBasic.FileSystem.Seek%2A>
- <xref:Microsoft.VisualBasic.FileSystem.Loc%2A>
