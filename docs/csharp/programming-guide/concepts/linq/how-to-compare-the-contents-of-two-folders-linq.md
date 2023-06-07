---
title: 如何比较两个文件夹的内容 (LINQ) (C#)
description: 此示例演示了使用 C# 中的 LINQ 查询来比较文件列表的三种方法。 使用这些方法来比较任何类型的对象序列。
ms.date: 07/20/2015
ms.assetid: c7c4870e-c500-4de3-afa4-2c8e07f510e6
ms.openlocfilehash: 840fda1548f36d9b9ad39c2e0d1346dfc727b106
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165255"
---
# <a name="how-to-compare-the-contents-of-two-folders-linq-c"></a>如何比较两个文件夹的内容 (LINQ) (C#)

此示例演示了比较两个文件列表的 3 种方法：  
  
- 通过查询布尔值指定两个文件列表是否相同。  
  
- 通过查询交集检索同时存在于两个文件夹中的文件。  
  
- 通过查询差集检索仅存在于一个文件夹中的文件。  
  
    > [!NOTE]
    > 此处的方法适用于比较任何类型的对象序列。  
  
 此处的 `FileComparer` 类演示如何将自定义比较器类与标准查询运算符结合使用。 此类不适合在实际方案中使用。 它仅使用每个文件的名称和字节长度来确定每个文件夹的内容是否相同。 在实际方案中，应修改此比较器以执行更严格的等同性检查。  
  
## <a name="example"></a>示例  
  
```csharp  
namespace QueryCompareTwoDirs  
{  
    class CompareDirs  
    {  
  
        static void Main(string[] args)  
        {  
  
            // Create two identical or different temporary folders
            // on a local drive and change these file paths.  
            string pathA = @"C:\TestDir";  
            string pathB = @"C:\TestDir2";  
  
            System.IO.DirectoryInfo dir1 = new System.IO.DirectoryInfo(pathA);  
            System.IO.DirectoryInfo dir2 = new System.IO.DirectoryInfo(pathB);  
  
            // Take a snapshot of the file system.  
            IEnumerable<System.IO.FileInfo> list1 = dir1.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
            IEnumerable<System.IO.FileInfo> list2 = dir2.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
            //A custom file comparer defined below  
            FileCompare myFileCompare = new FileCompare();  
  
            // This query determines whether the two folders contain  
            // identical file lists, based on the custom file comparer  
            // that is defined in the FileCompare class.  
            // The query executes immediately because it returns a bool.  
            bool areIdentical = list1.SequenceEqual(list2, myFileCompare);  
  
            if (areIdentical == true)  
            {  
                Console.WriteLine("the two folders are the same");  
            }  
            else  
            {  
                Console.WriteLine("The two folders are not the same");  
            }  
  
            // Find the common files. It produces a sequence and doesn't
            // execute until the foreach statement.  
            var queryCommonFiles = list1.Intersect(list2, myFileCompare);  
  
            if (queryCommonFiles.Any())  
            {  
                Console.WriteLine("The following files are in both folders:");  
                foreach (var v in queryCommonFiles)  
                {  
                    Console.WriteLine(v.FullName); //shows which items end up in result list  
                }  
            }  
            else  
            {  
                Console.WriteLine("There are no common files in the two folders.");  
            }  
  
            // Find the set difference between the two folders.  
            // For this example we only check one way.  
            var queryList1Only = (from file in list1  
                                  select file).Except(list2, myFileCompare);  
  
            Console.WriteLine("The following files are in list1 but not list2:");  
            foreach (var v in queryList1Only)  
            {  
                Console.WriteLine(v.FullName);  
            }  
  
            // Keep the console window open in debug mode.  
            Console.WriteLine("Press any key to exit.");  
            Console.ReadKey();  
        }  
    }  
  
    // This implementation defines a very simple comparison  
    // between two FileInfo objects. It only compares the name  
    // of the files being compared and their length in bytes.  
    class FileCompare : System.Collections.Generic.IEqualityComparer<System.IO.FileInfo>  
    {  
        public FileCompare() { }  
  
        public bool Equals(System.IO.FileInfo f1, System.IO.FileInfo f2)  
        {  
            return (f1.Name == f2.Name &&  
                    f1.Length == f2.Length);  
        }  
  
        // Return a hash that reflects the comparison criteria. According to the
        // rules for IEqualityComparer<T>, if Equals is true, then the hash codes must  
        // also be equal. Because equality as defined here is a simple value equality, not  
        // reference identity, it is possible that two or more objects will produce the same  
        // hash code.  
        public int GetHashCode(System.IO.FileInfo fi)  
        {  
            string s = $"{fi.Name}{fi.Length}";
            return s.GetHashCode();  
        }  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>编译代码  

 使用 System.Linq 和 System.IO 命名空间的 `using` 指令创建 C# 控制台应用程序项目。  
  
## <a name="see-also"></a>请参阅

- [LINQ to Objects (C#)](./linq-to-objects.md)
- [LINQ 和文件目录 (C#)](./linq-and-file-directories.md)
