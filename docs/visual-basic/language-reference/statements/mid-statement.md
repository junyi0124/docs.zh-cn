---
title: Mid 语句
ms.date: 07/20/2015
f1_keywords:
- vb.MidB
- vb.Mid
helpviewer_keywords:
- substrings [Visual Basic], Mid statement
- strings [Visual Basic], substrings
- Mid statement [Visual Basic]
- strings [Visual Basic], replacing
ms.assetid: 2b82d7a8-9646-4cb0-bec5-80abc98297bf
ms.openlocfilehash: 0379a6cabe819365b22994a5e4f9353d98b2c768
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873193"
---
# <a name="mid-statement"></a>Mid 语句

将变量中指定数量的字符替换 `String` 为另一个字符串中的字符。  
  
## <a name="syntax"></a>语法  
  
```vb  
Mid( _  
   ByRef Target As String, _  
   ByVal Start As Integer, _  
   Optional ByVal Length As Integer _  
) = StringExpression  
```  
  
## <a name="parts"></a>组成部分  

 `Target`  
 必需。 `String`要修改的变量的名称。  
  
 `Start`  
 必需。 `Integer` 表达式。 中 `Target` 开始替换文本的字符位置。 `Start` 使用从1开始的索引。  
  
 `Length`  
 可选。 `Integer` 表达式。 要替换的字符数。 如果省略， `String` 则使用所有。  
  
 `StringExpression`  
 必需。 `String` 替换部分的表达式 `Target` 。  
  
## <a name="exceptions"></a>异常  
  
|例外类型|条件|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|`Start` <= 0 或 `Length` < 0。|  
  
## <a name="remarks"></a>备注  

 替换的字符数始终小于或等于中的字符数 `Target` 。  
  
 Visual Basic 具有 <xref:Microsoft.VisualBasic.Strings.Mid%2A> 函数和 `Mid` 语句。 这些元素对字符串中的指定数量的字符进行操作，但 `Mid` 当 `Mid` 语句替换这些字符时，函数将返回字符。 有关详细信息，请参阅 <xref:Microsoft.VisualBasic.Strings.Mid%2A>。  
  
> [!NOTE]
> 的 `MidB` 早期 Visual Basic 版本语句将以字节而不是字符来替换子字符串（而不是字符）。 它主要用于在双字节字符集中将字符串转换 (DBCS) 应用程序。 所有 Visual Basic 字符串均采用 Unicode 格式， `MidB` 不再受支持。  
  
## <a name="example"></a>示例  

 此示例使用 `Mid` 语句将字符串变量中指定数量的字符替换为另一个字符串中的字符。  
  
 [!code-vb[VbVbalrStrings#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#5)]  
  
## <a name="requirements"></a>要求  

 **命名空间：** [Microsoft](../runtime-library-members.md)  
  
 **模块：**`Strings`  
  
 **程序集：** Microsoft.VisualBasic.dll) 中的 Visual Basic 运行时库 (  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Strings.Mid%2A>
- [字符串](../../programming-guide/language-features/strings/index.md)
- [字符串介绍 (Visual Basic)](../../programming-guide/language-features/strings/introduction-to-strings.md)
