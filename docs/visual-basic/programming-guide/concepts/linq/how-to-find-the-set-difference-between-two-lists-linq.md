---
title: 如何：查找两个列表之间的差集 (LINQ)
ms.date: 07/20/2015
ms.assetid: b5b25474-10a8-4df6-aab5-75621bb6b68e
ms.openlocfilehash: 1671cd32c0c0b8a3ff7fa6be87bd43dde9750776
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91100212"
---
# <a name="how-to-find-the-set-difference-between-two-lists-linq-visual-basic"></a>如何：查找两个列表之间的差集 (LINQ)  (Visual Basic) 

此示例演示如何使用 LINQ 对两个字符串列表进行比较，并输出那些位于 names1.txt 中但不在 names2.txt 中的行。  
  
### <a name="to-create-the-data-files"></a>创建数据文件  
  
1. 将 names1.txt 和 names2.txt 复制到解决方案文件夹中，如 [如何：结合和比较字符串集合 (LINQ)  (Visual Basic ](how-to-combine-and-compare-string-collections-linq.md)) 。  
  
## <a name="example"></a>示例  
  
```vb  
Class CompareLists  
  
    Shared Sub Main()  
  
        ' Create the IEnumerable data sources.  
        Dim names1 As String() = System.IO.File.ReadAllLines("../../../names1.txt")  
        Dim names2 As String() = System.IO.File.ReadAllLines("../../../names2.txt")  
  
        ' Create the query. Note that method syntax must be used here.  
        Dim differenceQuery = names1.Except(names2)  
        Console.WriteLine("The following lines are in names1.txt but not names2.txt")  
  
        ' Execute the query.  
        For Each name As String In differenceQuery  
            Console.WriteLine(name)  
        Next  
  
        ' Keep console window open in debug mode.  
        Console.WriteLine("Press any key to exit.")  
        Console.ReadKey()  
    End Sub  
End Class  
' Output:  
' The following lines are in names1.txt but not names2.txt  
' Potra, Cristina  
' Noriega, Fabricio  
' Aw, Kam Foo  
' Toyoshima, Tim  
' Guy, Wey Yuan  
' Garcia, Debra  
```  
  
 Visual Basic 中的某些类型的查询操作（例如 <xref:System.Linq.Enumerable.Except%2A> 、 <xref:System.Linq.Enumerable.Distinct%2A> 、 <xref:System.Linq.Enumerable.Union%2A> 和 <xref:System.Linq.Enumerable.Concat%2A> ）只能用基于方法的语法表示。  
  
## <a name="compile-the-code"></a>编译代码  

使用 `Imports` System. Linq 命名空间的语句创建 Visual Basic 控制台应用程序项目。
  
## <a name="see-also"></a>请参阅

- [LINQ 和字符串 (Visual Basic)](linq-and-strings.md)
