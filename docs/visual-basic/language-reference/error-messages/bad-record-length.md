---
title: 错误的记录长度
ms.date: 07/20/2015
f1_keywords:
- vbrID59
ms.assetid: 0926a3a4-177b-4452-9b33-d8a01e24cc21
ms.openlocfilehash: 6967015572b2567f52697f7ddcb1ff594013a2c4
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869264"
---
# <a name="bad-record-length"></a>错误的记录长度

此错误的可能原因包括：  
  
- 、或语句中指定的记录变量的 `FileGet` 长度 `FileGetObject` `FilePut` `FilePutObject` 与相应语句中指定的长度不同 `FileOpen` 。  
  
- 或语句中的 `FilePut` 变量 `FilePutObject` 为或包含可变长度字符串。  
  
- 或中的变量 `FilePut` `FilePutObject` 为或包含 `Variant` 类型。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 请确保定义记录变量类型的用户定义类型中的固定长度变量大小的总和与语句的子句中所述的值相同 `FileOpen` `Len` 。  
  
2. 如果或语句中的 `FilePut` 变量 `FilePutObject` 为或包含可变长度字符串，请确保可变长度字符串至少比语句的子句中指定的记录长度少2个字符 `Len` `FileOpen` 。  
  
3. 如果或中的变量 `FilePut` `FilePutObject` 为或包含， `Variant` 请确保长度可变的字符串至少比语句的子句中指定的记录长度少4个字节 `Len` `FileOpen` 。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileSystem.FileGet%2A>
- <xref:Microsoft.VisualBasic.FileSystem.FileGetObject%2A>
- <xref:Microsoft.VisualBasic.FileSystem.FilePut%2A>
- <xref:Microsoft.VisualBasic.FileSystem.FilePutObject%2A>
