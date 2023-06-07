---
title: 如何：查找程序集的完全限定名
description: 本文介绍如何获取 .NET 程序集的完全限定名称。
ms.date: 08/20/2019
helpviewer_keywords:
- names [.NET], fully qualified type names
- names [.NET], assemblies
- assemblies [.NET], names
ms.assetid: 009dae23-e1f6-4a64-9a9a-32e4c34802b0
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 223def7d992f5fae64c95aa6886f20980184eddc
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687621"
---
# <a name="how-to-find-an-assemblys-fully-qualified-name"></a>如何：查找程序集的完全限定名

若要在全局程序集缓存中查找某个 .NET Framework 程序集的完全限定名，请使用全局程序集缓存工具 ([Gacutil.exe](../../framework/tools/gacutil-exe-gac-tool.md))。 请参阅[如何：查看全局程序集缓存的内容](../../framework/app-domains/how-to-view-the-contents-of-the-gac.md)。

对于 .NET Core 程序集，以及对于不在全局程序集缓存中的 .NET Framework 程序集，可以通过多种方式获取完全限定的程序集名称：

- 可使用代码将信息输出到控制台或变量，或者使用 [Ildasm.exe (IL 反汇编程序)](../../framework/tools/ildasm-exe-il-disassembler.md) 检查程序集的元数据（其中包含了完全限定名）。

- 如果应用程序已加载程序集，则可检索 <xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType> 属性的值在以获取完全限定名。 可以使用该程序集中定义的 <xref:System.Type> 的 <xref:System.Type.Assembly> 属性来检索对 <xref:System.Reflection.Assembly> 对象的引用。 说明如示例所示。

- 如果知道程序集的文件系统路径，则可调用 `static` (C#) 或 `Shared` (Visual Basic) <xref:System.Reflection.AssemblyName.GetAssemblyName%2A?displayProperty=nameWithType> 方法获取完全限定的程序集名称。 下面是一个简单的示例。

  ```csharp
  using System;
  using System.Reflection;

  public class Example
  {
     public static void Main()
     {
        Console.WriteLine(AssemblyName.GetAssemblyName(@".\UtilityLibrary.dll"));
     }
  }
  // The example displays output like the following:
  //   UtilityLibrary, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null
  ```

  ```vb
  Imports System.Reflection

  Public Module Example
     Public Sub Main
        Console.WriteLine(AssemblyName.GetAssemblyName(".\UtilityLibrary.dll"))
     End Sub
  End Module
  ' The example displays output like the following:
  '   UtilityLibrary, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null
  ```

- 可使用 [Ildasm.exe（IL 反汇编程序）](../../framework/tools/ildasm-exe-il-disassembler.md)检查包含完全限定名的程序集元数据。

有关设置程序集属性（如版本、区域性和程序集名称）的详细信息，请参阅[设置程序集属性](set-attributes.md)。 有关为程序集提供强名称的详细信息，请参阅[创建并使用强名称程序集](create-use-strong-named.md)。

## <a name="example"></a>示例

下例演示了如何向控制台显示包含指定类的程序集的完全限定名。 它使用 <xref:System.Type.Assembly?displayProperty=nameWithType> 属性从该程序集中定义的类型检索对程序集的引用。

```cpp
#using <System.dll>
#using <System.Data.dll>

using namespace System;
using namespace System::Reflection;

ref class asmname
{
public:
    static void Main()
    {
        Type^ t = System::Data::DataSet::typeid;
        String^ s = t->Assembly->FullName->ToString();
        Console::WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s);
    }
};

int main()
{
    asmname::Main();
}
```

```csharp
using System;
using System.Reflection;

class asmname
{
    public static void Main()
    {
        Type t = typeof(System.Data.DataSet);
        string s = t.Assembly.FullName.ToString();
        Console.WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s);
    }
}
```

```vb
Imports System.Reflection

Class asmname
    Public Shared Sub Main()
        Dim t As Type = GetType(System.Data.DataSet)
        Dim s As String = t.Assembly.FullName.ToString()
        Console.WriteLine("The fully qualified assembly name " +
            "containing the specified class is {0}.", s)
    End Sub
End Class
```

## <a name="see-also"></a>请参阅

- [程序集名称](names.md)
- [创建程序集](create.md)
- [创建和使用具有强名称的程序集](create-use-strong-named.md)
- [全局程序集缓存](../../framework/app-domains/gac.md)
- [运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)
