---
title: 如何按任意词或字段对文本数据进行排序或筛选 (LINQ) (C#)
description: 了解如何按任意词或字段对文本数据进行排序或筛选。 参阅示例，了解如何按行中的任何字段对结构化文本行进行排序。
ms.date: 07/20/2015
ms.assetid: 7c04d42f-4a78-42c8-9ec8-57ef18fe13a9
ms.openlocfilehash: 05858cc787d3916b204910df10d3291796cebc02
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203938"
---
# <a name="how-to-sort-or-filter-text-data-by-any-word-or-field-linq-c"></a>如何按任意词或字段对文本数据进行排序或筛选 (LINQ) (C#)

下面的示例演示如何按行中的任何字段对结构化文本（如以逗号分隔的值）行进行排序。 可以在运行时动态指定字段。 假定 scores.csv 中的字段表示学生的 ID 号，后跟一系列四个测试分数。  
  
### <a name="to-create-a-file-that-contains-data"></a>创建包含数据的文件  
  
1. 从主题[如何联接不同文件的内容 (LINQ) (C#)](./how-to-join-content-from-dissimilar-files-linq.md) 复制 scores.csv 数据并将它保存到解决方案文件夹。  
  
## <a name="example"></a>示例  
  
```csharp  
public class SortLines  
{  
    static void Main()  
    {  
        // Create an IEnumerable data source  
        string[] scores = System.IO.File.ReadAllLines(@"../../../scores.csv");  
  
        // Change this to any value from 0 to 4.  
        int sortField = 1;  
  
        Console.WriteLine("Sorted highest to lowest by field [{0}]:", sortField);  
  
        // Demonstrates how to return query from a method.  
        // The query is executed here.  
        foreach (string str in RunQuery(scores, sortField))  
        {  
            Console.WriteLine(str);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    // Returns the query variable, not query results!  
    static IEnumerable<string> RunQuery(IEnumerable<string> source, int num)  
    {  
        // Split the string and sort on field[num]  
        var scoreQuery = from line in source  
                         let fields = line.Split(',')  
                         orderby fields[num] descending  
                         select line;  
  
        return scoreQuery;  
    }  
}  
/* Output (if sortField == 1):  
   Sorted highest to lowest by field [1]:  
    116, 99, 86, 90, 94  
    120, 99, 82, 81, 79  
    111, 97, 92, 81, 60  
    114, 97, 89, 85, 82  
    121, 96, 85, 91, 60  
    122, 94, 92, 91, 91  
    117, 93, 92, 80, 87  
    118, 92, 90, 83, 78  
    113, 88, 94, 65, 91  
    112, 75, 84, 91, 39  
    119, 68, 79, 88, 92  
    115, 35, 72, 91, 70  
 */  
```  
  
 此示例还演示如何从方法返回查询变量。  
  
## <a name="compiling-the-code"></a>编译代码  

使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。
  
## <a name="see-also"></a>请参阅

- [LINQ 和字符串 (C#)](./linq-and-strings.md)
