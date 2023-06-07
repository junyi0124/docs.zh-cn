---
title: 如何查询字符串中的字符 (LINQ) (C#)
description: 可以使用 LINQ 将字符串作为字符序列进行查询。 此 C# 示例查询一个字符串以确定它所包含的数字数量。
ms.date: 07/20/2015
ms.assetid: 727a1be7-dbec-4ab8-b414-bc2d56feb6ff
ms.openlocfilehash: 73288924d057e720a744b853998a52437b9db481
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153932"
---
# <a name="how-to-query-for-characters-in-a-string-linq-c"></a>如何查询字符串中的字符 (LINQ) (C#)

因为 <xref:System.String> 类可实现泛型 <xref:System.Collections.Generic.IEnumerable%601> 接口，因此任何字符串都可以字符序列的形式进行查询。 但是，这不是 LINQ 的一般用法。 对于复杂的模式匹配操作，请使用 <xref:System.Text.RegularExpressions.Regex> 类。  
  
## <a name="example"></a>示例  

 以下示例查询一个字符串以确定它所包含的数字数量。 请注意，在第一次执行此查询后将“重用”此查询。 这是可能的，因为查询本身并不存储任何实际的结果。  
  
```csharp  
class QueryAString  
{  
    static void Main()  
    {  
        string aString = "ABCDE99F-J74-12-89A";  
  
        // Select only those characters that are numbers  
        IEnumerable<char> stringQuery =  
          from ch in aString  
          where Char.IsDigit(ch)  
          select ch;  
  
        // Execute the query  
        foreach (char c in stringQuery)  
            Console.Write(c + " ");  
  
        // Call the Count method on the existing query.  
        int count = stringQuery.Count();  
        Console.WriteLine("Count = {0}", count);  
  
        // Select all characters before the first '-'  
        IEnumerable<char> stringQuery2 = aString.TakeWhile(c => c != '-');  
  
        // Execute the second query  
        foreach (char c in stringQuery2)  
            Console.Write(c);  
  
        Console.WriteLine(System.Environment.NewLine + "Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
  Output: 9 9 7 4 1 2 8 9  
  Count = 8  
  ABCDE99F  
*/  
```  
  
## <a name="compiling-the-code"></a>编译代码  

 使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。  
  
## <a name="see-also"></a>请参阅

- [LINQ 和字符串 (C#)](./linq-and-strings.md)
- [如何将 LINQ 查询与正则表达式合并 (C#)](./how-to-combine-linq-queries-with-regular-expressions.md)
