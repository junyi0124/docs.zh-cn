---
title: LINQ 和字符串
ms.date: 07/20/2015
ms.assetid: 75ddb201-d97a-4f98-8cdf-4ad51714529a
ms.openlocfilehash: 28c11dcc3c788ea85516e8b3fbafe2677b6d9b54
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075338"
---
# <a name="linq-and-strings-visual-basic"></a>LINQ 和字符串 (Visual Basic)

LINQ 可用于查询和转换字符串和字符串集合。 这在处理文本文件中的半结构化数据时尤其有用。 LINQ 查询可以与传统的字符串函数和正则表达式合并。 例如，可以使用 <xref:System.String.Split%2A> 或 <xref:System.Text.RegularExpressions.Regex.Split%2A> 方法来创建可稍后使用 LINQ 查询或修改的字符串数组。 可以使用 LINQ 查询的 `where` 子句中的 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> 方法。 并且可以使用 LINQ 查询或修改正则表达式返回的 <xref:System.Text.RegularExpressions.MatchCollection> 结果。  
  
 还可以使用本节介绍的技术将半结构化的文本数据转换为 XML。 有关详细信息，请参阅 [如何：从 CSV 文件生成 XML](../../../../standard/linq/generate-xml-csv-files.md)。  
  
 本节中的示例分为两类：  
  
## <a name="querying-a-block-of-text"></a>查询文本块  

 可以使用 <xref:System.String.Split%2A> 方法或 <xref:System.Text.RegularExpressions.Regex.Split%2A> 方法将文本块拆分为可查询的较小字符串数组，从而对其进行查询、分析和修改。 可以先将源文本拆分为词语、句、段落、页或任何其他条件，然后根据查询的需要执行其他拆分。  
  
 [如何：对某个词在字符串中出现的次数进行计数 (LINQ)  (Visual Basic) ](how-to-count-occurrences-of-a-word-in-a-string-linq.md)  
 演示如何使用 LINQ 进行简单文本查询。  
  
 [如何：查询包含一组指定词语的句子 (LINQ) (Visual Basic)](how-to-query-for-sentences-that-contain-a-specified-set-of-words.md)

 演示如何在任意边界上拆分文本文件以及如何针对每个部分执行查询。  
  
 [如何：查询字符串中的字符 (LINQ)  (Visual Basic) ](how-to-query-for-characters-in-a-string-linq.md)  
 演示字符串是可查询类型。  
  
 [如何将 LINQ 查询与正则表达式组合 (Visual Basic) ](how-to-combine-linq-queries-with-regular-expressions.md)  
 演示如何在 LINQ 查询中使用正则表达式，以便对筛选的查询结果进行复杂的模式匹配。  
  
## <a name="querying-semi-structured-data-in-text-format"></a>查询文本格式的半结构化数据  

 许多不同类型的文本文件都包含一系列行，通常具有类似的格式设置，例如制表符分隔或逗号分隔的文件或固定长度的行。 将此类文本文件读入内存后，可以使用 LINQ 来查询和/或修改其中的行。 LINQ 查询还简化了合并来自多个源的数据的任务。  
  
 [如何：查找两个列表之间的差集 (LINQ)  (Visual Basic) ](how-to-find-the-set-difference-between-two-lists-linq.md)  
 演示如何查找出现在一个列表中、但没有出现在另一个列表中的所有字符串。  
  
 [如何：按任意词或字段对文本数据进行排序或筛选 (LINQ) (Visual Basic)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)  
 演示如何基于任意词或字段对文本行进行排序。  
  
 [如何：重新排列带分隔符的文件的字段 (LINQ)  (Visual Basic) ](how-to-reorder-the-fields-of-a-delimited-file.md)  
 演示如何对 .csv 文件的某行中的字段进行重新排序。  
  
 [如何：合并和比较字符串集合 (LINQ)  (Visual Basic) ](how-to-combine-and-compare-string-collections-linq.md)  
 演示如何通过各种方式合并字符串列表。  
  
 [如何：从多个源 (LINQ)  (Visual Basic 填充对象集合) ](how-to-populate-object-collections-from-multiple-sources-linq.md)  
 演示如何将多个文本文件作为数据源来创建对象集合。  
  
 [如何： (LINQ)  (Visual Basic 联接不同文件的内容) ](how-to-join-content-from-dissimilar-files-linq.md)  
 演示如何使用匹配键将两个列表中的字符串合并成单个字符串。  
  
 [如何：使用组将一个文件拆分成多个文件 (LINQ)  (Visual Basic) ](how-to-split-a-file-into-many-files-by-using-groups-linq.md)  
 演示如何通过将单个文件用作数据源来创建新文件。  
  
 [如何：在 CSV 文本文件中计算列值 (LINQ)  (Visual Basic) ](how-to-compute-column-values-in-a-csv-text-file-linq.md)  
 演示如何在 .csv 文件中对文本数据执行数学计算。  
  
## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) (Visual Basic)](index.md)
- [如何：从 CSV 文件生成 XML](../../../../standard/linq/generate-xml-csv-files.md)
