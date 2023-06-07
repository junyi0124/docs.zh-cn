---
title: 如何：将字节数组转换成字符串
ms.date: 07/20/2015
helpviewer_keywords:
- string conversion [Visual Basic], arrays
- byte arrays [Visual Basic], converting to strings
- examples [Visual Basic], strings
- arrays [Visual Basic], converting to strings
ms.assetid: d0dc8317-9ab3-4324-99f7-3f5788c0e72a
ms.openlocfilehash: 49c1e454b845bd545d7a8dccb3a3d9a39ff2db21
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085654"
---
# <a name="how-to-convert-an-array-of-bytes-into-a-string-in-visual-basic"></a>如何：在 Visual Basic 中将字节数组转换为字符串

本主题说明如何将字节数组中的字节转换为字符串。  
  
## <a name="example"></a>示例  

 此示例使用 <xref:System.Text.Encoding.GetString%2A> encoding 类的方法 <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType> 将字节数组中的所有字节转换为字符串。  
  
 [!code-vb[VbVbalrStrings#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#72)]  
  
 可以从多个编码选项中进行选择，将字节数组转换为字符串：  
  
- <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType>：获取 ASCII (7 位) 字符集的编码。  
  
- <xref:System.Text.Encoding.BigEndianUnicode%2A?displayProperty=nameWithType>：获取使用大 endian 字节顺序的 UTF-16 格式的编码。  
  
- <xref:System.Text.Encoding.Default%2A?displayProperty=nameWithType>：获取系统的当前 ANSI 代码页的编码。  
  
- <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType>：获取使用小 endian 字节顺序的 UTF-16 格式的编码。  
  
- <xref:System.Text.Encoding.UTF32%2A?displayProperty=nameWithType>：使用小字节序字节顺序获取 UTF-32 格式的编码。  
  
- <xref:System.Text.Encoding.UTF7%2A?displayProperty=nameWithType>：获取 UTF-7 格式的编码。  
  
- <xref:System.Text.Encoding.UTF8%2A?displayProperty=nameWithType>：获取 UTF-8 格式的编码。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Text.Encoding?displayProperty=nameWithType>
- <xref:System.Text.Encoding.GetString%2A>
- [如何：在 Visual Basic 中将字符串转换为字节数组](how-to-convert-strings-into-an-array-of-bytes.md)
