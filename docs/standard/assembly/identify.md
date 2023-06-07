---
title: 如何：确定文件是否为程序集
description: 本文介绍如何以手动和编程方式确定文件是否是 .NET 程序集。
ms.date: 08/19/2019
ms.assetid: ea5186bb-5bff-4dcb-bde9-d6ba4e2edd00
dev_langs:
- csharp
- vb
ms.openlocfilehash: b78acaad31996f8fc2b965f51f541e99aeceb111
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731496"
---
# <a name="how-to-determine-if-a-file-is-an-assembly"></a>如何：确定文件是否为程序集

当且仅当程序集处于托管状态，并在其元数据中包含程序集条目时，该文件才为程序集。 有关程序集和元数据的详细信息，请参阅[程序集清单](manifest.md)。  
  
## <a name="how-to-manually-determine-if-a-file-is-an-assembly"></a>如何手动确定文件是否为程序集  
  
1. 启动 [Ildasm.exe（IL 反汇编程序）](../../framework/tools/ildasm-exe-il-disassembler.md)。  
  
2. 加载要测试的文件。  
  
3. 如果 **ILDASM** 报告文件不是可移植的可执行 (PE) 文件，则不是程序集。 有关详细信息，请参阅主题[如何：查看程序集内容](view-contents.md)。  
  
## <a name="how-to-programmatically-determine-if-a-file-is-an-assembly"></a>如何以编程方式确定文件是否为程序集  
  
1. 调用 <xref:System.Reflection.AssemblyName.GetAssemblyName%2A?displayProperty=nameWithType> 方法，传递要测试的文件的完整文件路径和名称。  
  
2. 如果引发 <xref:System.BadImageFormatException> 异常，则该文件不是程序集。  
  
## <a name="example"></a>示例  

此示例测试 DLL 以查看其是否为程序集。  

```csharp
class TestAssembly  
{  
    static void Main()  
    {  
  
        try  
        {  
            System.Reflection.AssemblyName testAssembly =  
                System.Reflection.AssemblyName.GetAssemblyName(@"C:\Windows\Microsoft.NET\Framework\v3.5\System.Net.dll");  
  
            System.Console.WriteLine("Yes, the file is an assembly.");  
        }  
  
        catch (System.IO.FileNotFoundException)  
        {  
            System.Console.WriteLine("The file cannot be found.");  
        }  
  
        catch (System.BadImageFormatException)  
        {  
            System.Console.WriteLine("The file is not an assembly.");  
        }  
  
        catch (System.IO.FileLoadException)  
        {  
            System.Console.WriteLine("The assembly has already been loaded.");  
        }  
    }  
}  
/* Output (with .NET Framework 3.5 installed):  
    Yes, the file is an assembly.  
*/  
```  

```vb  
Module Module1  
    Sub Main()  
        Try  
            Dim testAssembly As Reflection.AssemblyName =  
                                Reflection.AssemblyName.GetAssemblyName("C:\Windows\Microsoft.NET\Framework\v3.5\System.Net.dll")  
            Console.WriteLine("Yes, the file is an Assembly.")  
        Catch ex As System.IO.FileNotFoundException  
            Console.WriteLine("The file cannot be found.")  
        Catch ex As System.BadImageFormatException  
            Console.WriteLine("The file is not an Assembly.")  
        Catch ex As System.IO.FileLoadException  
            Console.WriteLine("The Assembly has already been loaded.")  
        End Try  
        Console.ReadLine()  
    End Sub  
End Module  
' Output (with .NET Framework 3.5 installed):  
'        Yes, the file is an Assembly.  
```

<xref:System.Reflection.AssemblyName.GetAssemblyName%2A> 方法加载测试文件，然后在读取信息之后释放它。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Reflection.AssemblyName>
- [C# 编程指南](../../csharp/programming-guide/index.md)
- [编程概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
- [.NET 中的程序集](index.md)
