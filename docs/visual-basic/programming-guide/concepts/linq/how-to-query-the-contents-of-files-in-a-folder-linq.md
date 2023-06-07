---
title: 如何：查询文件夹中的文件的内容 (LINQ)
ms.date: 07/20/2015
ms.assetid: edacbcd3-f3e4-4429-a8be-28a58dc0dd70
ms.openlocfilehash: 3ad5fd6c893d590d46be67e6320ac5b915829f4b
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346036"
---
# <a name="how-to-query-the-contents-of-files-in-a-folder-linq-visual-basic"></a>如何查询文件夹中文件的内容（LINQ）（Visual Basic）

此示例演示如何查询指定目录树中的所有文件、打开每个文件并检查其内容。 此类技术可用于对目录树的内容创建索引或反向索引。 此示例中执行的是简单的字符串搜索。 但是，可使用正则表达式执行类型更复杂的模式匹配。 有关详细信息，请参阅[如何：将 LINQ 查询与正则表达式合并（Visual Basic）](how-to-combine-linq-queries-with-regular-expressions.md)。  
  
## <a name="example"></a>示例  
  
```vb
Imports System.IO

Module Module1  
    'QueryContents  
    Public Sub Main()  
  
        ' Modify this path as necessary.  
        Dim startFolder = "C:\Program Files (x86)\Microsoft Visual Studio 14.0"  

        ' Take a snapshot of the folder contents.
        Dim dir As New DirectoryInfo(startFolder)
        Dim fileList = dir.GetFiles("*.*", SearchOption.AllDirectories)

        Dim searchTerm = "Welcome"

        ' Search the contents of each file.
        ' A regular expression created with the RegEx class
        ' could be used instead of the Contains method.
        Dim queryMatchingFiles = From file In fileList _
                                 Where file.Extension = ".html" _
                                 Let fileText = GetFileText(file.FullName) _
                                 Where fileText.Contains(searchTerm) _
                                 Select file.FullName

        Console.WriteLine("The term " & searchTerm & " was found in:")

        ' Execute the query.
        For Each filename In queryMatchingFiles
            Console.WriteLine(filename)
        Next

        ' Keep the console window open in debug mode.
        Console.WriteLine("Press any key to exit")
        Console.ReadKey()

    End Sub

    ' Read the contents of the file. This is done in a separate
    ' function in order to handle potential file system errors.
    Function GetFileText(name As String) As String

        ' If the file has been deleted, the right thing
        ' to do in this case is return an empty string.
        Dim fileContents = String.Empty

        ' If the file has been deleted since we took
        ' the snapshot, ignore it and return the empty string.
        If File.Exists(name) Then
            fileContents = File.ReadAllText(name)
        End If

        Return fileContents

    End Function
End Module
```

## <a name="compile-the-code"></a>编译代码

创建 Visual Basic 的控制台应用程序项目，复制并粘贴代码示例，并调整项目属性中的启动对象值。

## <a name="see-also"></a>另请参阅

- [LINQ to Objects (Visual Basic)](linq-to-objects.md)
- [LINQ 和文件目录 (Visual Basic)](linq-and-file-directories.md)
