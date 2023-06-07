---
title: 如何查询具有指定特性或名称的文件 (C#)
description: 了解如何使用 C# 中的 LINQ 在目录树中查找具有指定文件扩展名的文件，以及如何返回最新或最旧的文件。
ms.date: 07/20/2015
ms.assetid: 560e3879-b0b3-4549-ad02-0a53aff2f83c
ms.openlocfilehash: 01a3482d8ea4c95b60dd9434320f175f0498c3e8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165307"
---
# <a name="how-to-query-for-files-with-a-specified-attribute-or-name-c"></a>如何查询具有指定特性或名称的文件 (C#)

此示例演示了如何在指定目录树中查找具有指定文件扩展名（如“.txt”）的所有文件。 它还演示了如何基于时间在树中返回最新或最旧的文件。  
  
## <a name="example"></a>示例  
  
```csharp  
class FindFileByExtension  
{  
    // This query will produce the full path for all .txt files  
    // under the specified folder including subfolders.  
    // It orders the list according to the file name.  
    static void Main()  
    {  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        //Create the query  
        IEnumerable<System.IO.FileInfo> fileQuery =  
            from file in fileList  
            where file.Extension == ".txt"  
            orderby file.Name  
            select file;  
  
        //Execute the query. This might write out a lot of files!  
        foreach (System.IO.FileInfo fi in fileQuery)  
        {  
            Console.WriteLine(fi.FullName);  
        }  
  
        // Create and execute a new query by using the previous
        // query as a starting point. fileQuery is not
        // executed again until the call to Last()  
        var newestFile =  
            (from file in fileQuery  
             orderby file.CreationTime  
             select new { file.FullName, file.CreationTime })  
            .Last();  
  
        Console.WriteLine("\r\nThe newest .txt file is {0}. Creation time: {1}",  
            newestFile.FullName, newestFile.CreationTime);  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>编译代码  

  使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。
  
## <a name="see-also"></a>请参阅

- [LINQ to Objects (C#)](./linq-to-objects.md)
- [LINQ 和文件目录 (C#)](./linq-and-file-directories.md)
