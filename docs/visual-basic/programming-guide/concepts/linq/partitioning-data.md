---
title: 数据分区
ms.date: 07/20/2015
ms.assetid: 69c59379-b66e-422c-b324-5b5c07760ef7
ms.openlocfilehash: 07e33c4ce0d062a0f083d8970c0ad01f98923a52
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075312"
---
# <a name="partitioning-data-visual-basic"></a>将数据分区 (Visual Basic) 

LINQ 中的分区是指将输入序列划分为两个部分的操作，无需重新排列元素，然后返回其中一个部分。  
  
 下图显示对字符序列进行三种不同的分区操作的结果。 第一个操作返回序列中的前三个元素。 第二个操作跳过前三个元素，返回剩余元素。 第三个操作跳过序列中的前两个元素，返回接下来的三个元素。  
  
 ![显示三个 LINQ 分区操作的图示。](./media/partitioning-data/linq-partitioning-operations.png)  
  
 下面一节列出了对序列进行分区的标准查询运算符方法。  
  
## <a name="operators"></a>运算符  
  
|运算符名称|描述|Visual Basic 查询表达式语法|更多信息|  
|-------------------|-----------------|------------------------------------------|----------------------|  
|Skip|跳过序列中指定位置之前的元素。|`Skip`|<xref:System.Linq.Enumerable.Skip%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Skip%2A?displayProperty=nameWithType>|  
|SkipWhile|基于谓词函数跳过元素，直到元素不符合条件。|`Skip While`|<xref:System.Linq.Enumerable.SkipWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SkipWhile%2A?displayProperty=nameWithType>|  
|Take|获取序列中指定位置之前的元素。|`Take`|<xref:System.Linq.Enumerable.Take%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Take%2A?displayProperty=nameWithType>|  
|TakeWhile|基于谓词函数获取元素，直到元素不符合条件。|`Take While`|<xref:System.Linq.Enumerable.TakeWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.TakeWhile%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>查询表达式语法示例  
  
### <a name="skip"></a>跳过  

 下面的代码示例使用 `Skip` Visual Basic 中的子句在返回数组中的剩余字符串之前，跳过字符串数组中的前四个字符串。  
  
 [!code-vb[CsLINQPartitioning#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#1)]  
  
### <a name="skipwhile"></a>SkipWhile  

 下面的代码示例使用 `Skip While` Visual Basic 中的子句跳过数组中的字符串，而字符串的第一个字母为 "a"。 返回数组中的剩余字符串。  
  
 [!code-vb[CsLINQPartitioning#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#2)]  
  
### <a name="take"></a>Take  

 下面的代码示例使用 `Take` Visual Basic 中的子句来返回字符串数组中的前两个字符串。  
  
 [!code-vb[CsLINQPartitioning#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#3)]  
  
### <a name="takewhile"></a>TakeWhile  

 下面的代码示例使用 `Take While` Visual Basic 中的子句来返回数组中的字符串，而字符串的长度为5个或更少。  
  
 [!code-vb[CsLINQPartitioning#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#4)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [Skip 子句](../../../language-reference/queries/skip-clause.md)
- [Skip While 子句](../../../language-reference/queries/skip-while-clause.md)
- [Take 子句](../../../language-reference/queries/take-clause.md)
- [Take While 子句](../../../language-reference/queries/take-while-clause.md)
