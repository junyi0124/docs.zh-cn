---
title: 如何重新排列带分隔符的文件的字段 (LINQ) (C#)
description: 了解如何使用 C# 中的 LINQ 在 .csv 文件中重新排列字段。 此示例将更改列的顺序，合并到列并按列值对行进行排序。
ms.date: 07/20/2015
ms.assetid: 4e62d82c-61b7-4f18-b9a1-86723746d7d2
ms.openlocfilehash: 674e6a62112e17107eff690d7656f52488cd08c1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203951"
---
# <a name="how-to-reorder-the-fields-of-a-delimited-file-linq-c"></a>如何重新排列带分隔符的文件的字段 (LINQ) (C#)

逗号分隔值 (CSV) 文件是一种文本文件，通常用于存储电子表格数据或其他由行和列表示的表格数据。 通过使用 <xref:System.String.Split%2A> 方法分隔字段，可以非常轻松地使用 LINQ 来查询和操作 CSV 文件。 事实上，可以使用此技术来重新排列任何结构化文本行部分；此技术不局限于 CSV 文件。  
  
 在下面的示例中，假设有三列分别代表学生的“姓氏”、“名字”和“ID”。 这些字段基于学生的姓氏按字母顺序排列。 查询生成一个新序列，其中首先出现的是 ID 列，后面的第二列组合了学生的名字和姓氏。 根据 ID 字段重新排列各行。 结果保存到新文件，但不修改原始数据。  
  
### <a name="to-create-the-data-file"></a>创建数据文件  
  
1. 将以下各行复制到名为 spreadsheet1.csv 的纯文本文件。 将此文件保存到项目文件夹。  
  
    ```csv  
    Adams,Terry,120  
    Fakhouri,Fadi,116  
    Feng,Hanying,117  
    Garcia,Cesar,114  
    Garcia,Debra,115  
    Garcia,Hugo,118  
    Mortensen,Sven,113  
    O'Donnell,Claire,112  
    Omelchenko,Svetlana,111  
    Tucker,Lance,119  
    Tucker,Michael,122  
    Zabokritski,Eugene,121  
    ```  
  
## <a name="example"></a>示例  
  
```csharp  
class CSVFiles  
{  
    static void Main(string[] args)  
    {  
        // Create the IEnumerable data source  
        string[] lines = System.IO.File.ReadAllLines(@"../../../spreadsheet1.csv");  
  
        // Create the query. Put field 2 first, then  
        // reverse and combine fields 0 and 1 from the old field  
        IEnumerable<string> query =  
            from line in lines  
            let x = line.Split(',')  
            orderby x[2]  
            select x[2] + ", " + (x[1] + " " + x[0]);  
  
        // Execute the query and write out the new file. Note that WriteAllLines  
        // takes a string[], so ToArray is called on the query.  
        System.IO.File.WriteAllLines(@"../../../spreadsheet2.csv", query.ToArray());  
  
        Console.WriteLine("Spreadsheet2.csv written to disk. Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output to spreadsheet2.csv:  
111, Svetlana Omelchenko  
112, Claire O'Donnell  
113, Sven Mortensen  
114, Cesar Garcia  
115, Debra Garcia  
116, Fadi Fakhouri  
117, Hanying Feng  
118, Hugo Garcia  
119, Lance Tucker  
120, Terry Adams  
121, Eugene Zabokritski  
122, Michael Tucker  
 */  
```  
  
## <a name="compiling-the-code"></a>编译代码  

使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。
  
## <a name="see-also"></a>请参阅

- [LINQ 和字符串 (C#)](./linq-and-strings.md)
- [LINQ 和文件目录 (C#)](./linq-and-file-directories.md)
- [如何从 CSV 文件生成 XML (C#)](../../../../standard/linq/generate-xml-csv-files.md)
