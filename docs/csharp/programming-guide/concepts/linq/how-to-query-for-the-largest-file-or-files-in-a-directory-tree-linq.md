---
title: 如何查询目录树中的一个或多个最大的文件 (LINQ) (C#)
description: 此 C# 示例演示与文件大小（以字节为单位）相关的五个 LINQ 查询。 你可以修改它们以查询 FileInfo 对象的其他某个属性。
ms.date: 07/20/2015
ms.assetid: 20c8a917-0552-4514-b489-0b8b6a4c3b4c
ms.openlocfilehash: 049db9bf104af1593ba9807c307008e8e760da32
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176248"
---
# <a name="how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq-c"></a>如何查询目录树中的一个或多个最大的文件 (LINQ) (C#)

此示例演示与文件大小（以字节为单位）相关的五个查询：  
  
- 如何检索最大文件的大小（以字节为单位）。  
  
- 如何检索最小文件的大小（以字节为单位）。  
  
- 如何从指定根文件夹下的一个或多个文件夹检索 <xref:System.IO.FileInfo> 对象最大或最小文件。  
  
- 如何检索序列（如 10 个最大文件）。  
  
- 如何基于文件大小（以字节为单位）按组对文件进行排序（忽略小于指定大小的文件）。  
  
## <a name="example"></a>示例  

 下面的示例包含五个单独的查询，它们演示如何根据文件大小（以字节为单位）对文件进行查询和分组。 可以轻松地修改这些示例，以便使查询基于 <xref:System.IO.FileInfo> 对象的其他某个属性。  
  
```csharp  
class QueryBySize  
{  
    static void Main(string[] args)  
    {  
        QueryFilesBySize();  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    private static void QueryFilesBySize()  
    {  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        //Return the size of the largest file  
        long maxSize =  
            (from file in fileList  
             let len = GetFileLength(file)  
             select len)  
             .Max();  
  
        Console.WriteLine("The length of the largest file under {0} is {1}",  
            startFolder, maxSize);  
  
        // Return the FileInfo object for the largest file  
        // by sorting and selecting from beginning of list  
        System.IO.FileInfo longestFile =  
            (from file in fileList  
             let len = GetFileLength(file)  
             where len > 0  
             orderby len descending  
             select file)  
            .First();  
  
        Console.WriteLine("The largest file under {0} is {1} with a length of {2} bytes",  
                            startFolder, longestFile.FullName, longestFile.Length);  
  
        //Return the FileInfo of the smallest file  
        System.IO.FileInfo smallestFile =  
            (from file in fileList  
             let len = GetFileLength(file)  
             where len > 0  
             orderby len ascending  
             select file).First();  
  
        Console.WriteLine("The smallest file under {0} is {1} with a length of {2} bytes",  
                            startFolder, smallestFile.FullName, smallestFile.Length);  
  
        //Return the FileInfos for the 10 largest files  
        // queryTenLargest is an IEnumerable<System.IO.FileInfo>  
        var queryTenLargest =  
            (from file in fileList  
             let len = GetFileLength(file)  
             orderby len descending  
             select file).Take(10);  
  
        Console.WriteLine("The 10 largest files under {0} are:", startFolder);  
  
        foreach (var v in queryTenLargest)  
        {  
            Console.WriteLine("{0}: {1} bytes", v.FullName, v.Length);  
        }  
  
        // Group the files according to their size, leaving out  
        // files that are less than 200000 bytes.
        var querySizeGroups =  
            from file in fileList  
            let len = GetFileLength(file)  
            where len > 0  
            group file by (len / 100000) into fileGroup  
            where fileGroup.Key >= 2  
            orderby fileGroup.Key descending  
            select fileGroup;  
  
        foreach (var filegroup in querySizeGroups)  
        {  
            Console.WriteLine(filegroup.Key.ToString() + "00000");  
            foreach (var item in filegroup)  
            {  
                Console.WriteLine("\t{0}: {1}", item.Name, item.Length);  
            }  
        }  
    }  
  
    // This method is used to swallow the possible exception  
    // that can be raised when accessing the FileInfo.Length property.  
    // In this particular case, it is safe to swallow the exception.  
    static long GetFileLength(System.IO.FileInfo fi)  
    {  
        long retval;  
        try  
        {  
            retval = fi.Length;  
        }  
        catch (System.IO.FileNotFoundException)  
        {  
            // If a file is no longer present,  
            // just add zero bytes to the total.  
            retval = 0;  
        }  
        return retval;  
    }  
  
}  
```  
  
 若要返回一个或多个完整的 <xref:System.IO.FileInfo> 对象，查询必须首先检查数据中的每个对象，然后按其 Length 属性值对它们进行排序。 随后它便可以返回具有最大长度的单个对象或对象序列。 使用 <xref:System.Linq.Enumerable.First%2A> 返回列表中的第一个元素。 使用 <xref:System.Linq.Enumerable.Take%2A> 返回前 n 个元素。 指定降序排序顺序可将最小元素置于列表开头。  
  
 查询调用单独的方法来获取文件大小（以字节为单位），以便使用在以下情况下会引发的可能异常：自在 `GetFiles` 调用中创建了 <xref:System.IO.FileInfo> 对象以来的时间段内，在其他线程中删除了文件。 即使创建了 <xref:System.IO.FileInfo> 对象，该异常也可能出现，因为 <xref:System.IO.FileInfo> 对象会在首次访问其 <xref:System.IO.FileInfo.Length%2A> 属性时，尝试使用最新大小（以字节为单位）刷新该属性。 通过将此操作置于查询外部的 try-catch 块中，我们可遵循在查询中避免可能导致副作用的操作这一规则。 一般情况下，在使用异常时必须格外谨慎，以确保应用程序不会处于未知状态。  
  
## <a name="compiling-the-code"></a>编译代码  

使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。

## <a name="see-also"></a>请参阅

- [LINQ to Objects (C#)](./linq-to-objects.md)
- [LINQ 和文件目录 (C#)](./linq-and-file-directories.md)
