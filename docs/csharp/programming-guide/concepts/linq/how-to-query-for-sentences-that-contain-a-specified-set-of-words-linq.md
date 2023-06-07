---
title: 如何查询包含一组指定词语的句子 (LINQ) (C#)
description: 了解如何使用 C# 中的 LINQ 在包含一组词语的每个匹配项的文本文件中查找句子，这些词语可在运行时进行填充。
ms.date: 07/20/2015
ms.assetid: 0724b429-4b87-4d26-a7b1-409358f3fc20
ms.openlocfilehash: 7c5979fc42b6446948ae974e599807b5cb8db04a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176274"
---
# <a name="how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq-c"></a>如何查询包含一组指定词语的句子 (LINQ) (C#)

此示例演示如何在包含一组指定的词语的每个匹配项的文本文件中查找句子。 尽管此示例中的搜索词数组采用硬编码形式，但它也可在运行时以动态方式进行填充。 在此示例中，查询将返回包含单词“Historically,”、“data,”和“integrated”的句子。  
  
## <a name="example"></a>示例  
  
```csharp  
class FindSentences  
{  
    static void Main()  
    {  
        string text = @"Historically, the world of data and the world of objects " +  
        @"have not been well integrated. Programmers work in C# or Visual Basic " +  
        @"and also in SQL or XQuery. On the one side are concepts such as classes, " +  
        @"objects, fields, inheritance, and .NET APIs. On the other side " +  
        @"are tables, columns, rows, nodes, and separate languages for dealing with " +  
        @"them. Data types often require translation between the two worlds; there are " +  
        @"different standard functions. Because the object world has no notion of query, a " +  
        @"query can only be represented as a string without compile-time type checking or " +  
        @"IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to " +  
        @"objects in memory is often tedious and error-prone.";  
  
        // Split the text block into an array of sentences.  
        string[] sentences = text.Split(new char[] { '.', '?', '!' });  
  
        // Define the search terms. This list could also be dynamically populated at runtime.  
        string[] wordsToMatch = { "Historically", "data", "integrated" };  
  
        // Find sentences that contain all the terms in the wordsToMatch array.  
        // Note that the number of terms to match is not specified at compile time.  
        var sentenceQuery = from sentence in sentences  
                            let w = sentence.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' },  
                                                    StringSplitOptions.RemoveEmptyEntries)  
                            where w.Distinct().Intersect(wordsToMatch).Count() == wordsToMatch.Count()  
                            select sentence;  
  
        // Execute the query. Note that you can explicitly type  
        // the iteration variable here even though sentenceQuery  
        // was implicitly typed.
        foreach (string str in sentenceQuery)  
        {  
            Console.WriteLine(str);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
Historically, the world of data and the world of objects have not been well integrated  
*/  
```  
  
 查询运行时首先将文本拆分成句子，然后将句子拆分成包含每个单词的字符串数组。 对于每个数组，<xref:System.Linq.Enumerable.Distinct%2A> 方法将删除所有重复字词，然后查询将对字词数组和 `wordsToMatch` 数组执行 <xref:System.Linq.Enumerable.Intersect%2A> 操作。 如果相交数与 `wordsToMatch` 数组的计数相同，将在单词中找到所有单词并返回原始句子。  
  
 在对 <xref:System.String.Split%2A> 的调用中，使用标点符号作为分隔符，以从字符串中删除标点符号。 如果你没有不这样做，则假如你有一个字符串 “Historically,”，该字符串不会与 `wordsToMatch` 数组中的“Historically”匹配。 根据在源文本中找到的标点类型，可能需要使用其他分隔符。  
  
## <a name="compiling-the-code"></a>编译代码  

使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。

## <a name="see-also"></a>请参阅

- [LINQ 和字符串 (C#)](./linq-and-strings.md)
