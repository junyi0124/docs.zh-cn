---
title: 如何查询文件夹中文本文件的内容 (LINQ) (C#)
description: 了解如何使用 C# 中的 LINQ 查询目录树中的所有文件，打开每个文件并检查其内容。
ms.date: 07/20/2015
ms.assetid: f5b4dce7-1a34-4eb4-9bf1-60d5bdda264c
ms.openlocfilehash: a1f3e29751cd91ac1fd8e6601aa078d967776f5a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91159015"
---
# <a name="how-to-query-the-contents-of-text-files-in-a-folder-linq-c"></a>如何查询文件夹中文本文件的内容 (LINQ) (C#)

此示例演示如何查询指定目录树中的所有文件、打开每个文件并检查其内容。 此类技术可用于对目录树的内容创建索引或反向索引。 此示例中执行的是简单的字符串搜索。 但是，可使用正则表达式执行类型更复杂的模式匹配。 有关详细信息，请参阅[如何将 LINQ 查询与正则表达式合并 (C#)](./how-to-combine-linq-queries-with-regular-expressions.md)。  
  
## <a name="example"></a>示例  
  
```csharp  
class QueryContents  
{  
    public static void Main()  
    {  
        // Modify this path as necessary.  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        string searchTerm = @"Visual Studio";  
  
        // Search the contents of each file.  
        // A regular expression created with the RegEx class  
        // could be used instead of the Contains method.  
        // queryMatchingFiles is an IEnumerable<string>.  
        var queryMatchingFiles =  
            from file in fileList  
            where file.Extension == ".htm"  
            let fileText = GetFileText(file.FullName)  
            where fileText.Contains(searchTerm)  
            select file.FullName;  
  
        // Execute the query.  
        Console.WriteLine("The term \"{0}\" was found in:", searchTerm);  
        foreach (string filename in queryMatchingFiles)  
        {  
            Console.WriteLine(filename);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    // Read the contents of the file.  
    static string GetFileText(string name)  
    {  
        string fileContents = String.Empty;  
  
        // If the file has been deleted since we took
        // the snapshot, ignore it and return the empty string.  
        if (System.IO.File.Exists(name))  
        {  
            fileContents = System.IO.File.ReadAllText(name);  
        }  
        return fileContents;  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>编译代码  

使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。
  
## <a name="see-also"></a>请参阅

- [LINQ 和文件目录 (C#)](./linq-and-file-directories.md)
- [LINQ to Objects (C#)](./linq-to-objects.md)
