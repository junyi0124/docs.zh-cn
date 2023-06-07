---
title: 使用 Assert 方法
description: 请参阅如何使用 Assert 方法启用代码 (和下游调用方的代码) ，以执行代码有权访问而不是其调用方。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- granting permissions, overriding security checks
- code access security, overriding security checks
- overriding security checks
- security [.NET Framework], overriding security checks
- security [.NET Framework], assertions
- asserted permissions
- Assert method
- caller security checks
- permissions [.NET Framework], overriding security checks
- permissions [.NET Framework], assertions
ms.assetid: 1e40f4d3-fb7d-4f19-b334-b6076d469ea9
ms.openlocfilehash: 573b84f991e795c2513f213ddb52999fef51c454
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855657"
---
# <a name="using-the-assert-method"></a>使用 Assert 方法

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 <xref:System.Security.CodeAccessPermission.Assert%2A> 是一种可在代码访问权限类和 <xref:System.Security.PermissionSet> 类上调用的方法。 您可以使用**Assert**来使您的代码 (和下游调用方) 执行您的代码有权执行的操作，但其调用方可能无权执行此操作。 安全断言会更改在安全检查期间运行时所执行的正常过程。 断言权限时，它会通知安全系统不检查断言权限的代码的调用方。  
  
> [!CAUTION]
> 请谨慎使用断言，因为它们会打开安全漏洞并破坏运行时强制执行安全限制的机制。  
  
 当库调用非托管代码或所执行的调用需要与库的预期用途不明显关联的权限时，断言非常有用。 例如，调用到非托管代码的所有托管代码都必须具有指定了**UnmanagedCode**标志的**SecurityPermission** 。 默认情况下，如果代码不是来自本地计算机（如从本地 intranet 下载的代码），则不会对其授予此权限。 因此，为了使从本地 intranet 下载的代码能调用使用非托管代码的库，它必须具有由该库断言的权限。 此外，某些库可能会执行对调用方不可见且需要特殊权限的调用。  
  
 在代码以对调用方完全不可见的方式访问资源时，也可使用断言。 例如，假设你的库从数据库获取信息，但在此过程中还会从计算机注册表读取信息。 由于使用您的库的开发人员不能访问您的源，因此他们无法知道其代码需要**system.security.permissions.registrypermission>** 才能使用您的代码。 在此情况下，如果你决定要求代码调用方有权访问注册表是不合理或不必要的，你可以断言权限以读取注册表。 在这种情况下，适合库断言权限，使没有**system.security.permissions.registrypermission>** 的调用方可以使用库。  
  
 仅当断言权限和下游调用方所需的权限属于同一类型且要求的权限是断言权限的子集时，断言才会影响堆栈审核。 例如，如果你将**FileIOPermission**声明为读取 C 驱动器上的所有文件，并且**FileIOPermission**读取 C：\Temp 中的文件，则断言可能会影响堆栈审核;但是，如果要求**FileIOPermission**写入 C 驱动器，则断言不起作用。  
  
 若要执行断言，必须向你的代码授予正在断言的权限以及表示有权执行断言的 <xref:System.Security.Permissions.SecurityPermission>。 虽然你可以断言尚未授予代码的权限，但此断言毫无意义，因为安全检查在断言使其成功之前就会失败。  
  
 下图显示了使用**Assert**时会发生的情况。 假设以下语句针对程序集 A、B、C、E 和 F 以及 P1 和 P1A 两个权限为 true：  
  
- P1A 表示读取 C 驱动器上的 .txt 文件的权限。  
  
- P1 表示读取 C 驱动器上所有文件的权限。  
  
- P1A 和 P1 均为**FileIOPermission**类型，而 P1A 是 P1 的子集。  
  
- 已向程序集 E 和 F 授予了 P1A 权限。  
  
- 已向程序集 C 授予了 P1 权限。  
  
- 尚未向程序集 A 和 B 授予 P1 或 P1A 权限。  
  
- 方法 A 包含在程序集 A 中，方法 B 包含在程序集 B 中，依次类推。  
  
 ![显示断言方法程序集的关系图。](./media/using-the-assert-method/assert-method-assemblies.gif)
  
 在这种情况下，方法 A 调用 B，B 调用 C，C 调用 E，E 调用 F。方法 C 断言权限读取 C 驱动器上的文件 (权限 P1) ，而方法 E 要求权限读取 C 驱动器上的 .txt 文件 (权限 P1A) 。 当在运行时遇到 F 中的请求时，将执行堆栈遍历来检查 F 的所有调用方的权限，从 E. E 获得 P1A 权限，因此堆栈审核会继续检查 C 的权限，其中发现 C 的断言。 因为要求的权限 (P1A) 是断言的权限 (P1) 的子集，所以堆栈审核将停止且安全检查会自动成功。 未向程序集 A 和 B 授予权限 P1A 并不重要。 通过断言 P1，方法 C 确保其调用方可以访问受 P1 保护的资源，即使尚未授予调用方访问该资源的权限也是如此。  
  
 如果你设计一个类库且某个类访问受保护的资源，则在大多数情况下，你应该提出安全要求，要求此类的调用方拥有适当的权限。 如果类随后执行你知道其大多数调用方不具有权限的操作，并且你愿意承担让这些调用方调用你的代码的责任，则可以通过对表示代码正在执行的操作的权限对象调用**assert**方法来断言权限。 以这种方式使用**断言**可以使通常不能调用你的代码的调用方。 因此，如果你断言了一个权限，应确保预先执行了适当的安全检查，以防止你的组件被误用。  
  
 例如，假设你的高度受信任的库类具有删除文件的方法。 它通过调用非托管的 Win32 函数来访问文件。 调用方调用你的代码的**Delete**方法，并传入要删除的文件的名称，C:\Test.txt。 在**Delete**方法中，你的代码将创建一个对象，该 <xref:System.Security.Permissions.FileIOPermission> 对象表示对 C:\Test.txt 的写访问。 需要 (写入访问权限才能删除文件。 ) 你的代码，通过调用**FileIOPermission**对象的**要求**方法来调用命令性安全检查。 如果调用堆栈中的某个调用方不具有此权限，则会引发 <xref:System.Security.SecurityException>。 如果未引发任何异常，你会知道所有调用方都有权限访问 C:\Test.txt。 由于你认为大多数调用方不具有访问非托管代码的权限，因此你的代码会创建一个 <xref:System.Security.Permissions.SecurityPermission> 对象，该对象表示调用非托管代码和调用该对象的**断言**方法的权限。 最后，它会调用非托管的 Win32 函数来删除 C:\Text.txt，并将控件返回给调用方。  
  
> [!CAUTION]
> 你必须确保你的代码在以下情况下不使用断言：其他代码可使用你的代码来访问由你正在断言的权限所保护的资源。 例如，在写入文件（其名称由调用方指定为参数）的代码中，你将不会断言**FileIOPermission**来写入文件，因为你的代码将被第三方滥用。  
  
 使用命令性安全语法时，对同一方法中的多个权限调用**Assert**方法会引发安全异常。 相反，应创建**PermissionSet**对象，向其传递要调用的单个权限，然后调用**PermissionSet**对象上的**Assert**方法。 使用声明性安全语法时，可以多次调用**断言**方法。  
  
 下面的示例演示了使用**Assert**方法重写安全检查的声明性语法。 请注意，**来**语法采用两个值：一个 <xref:System.Security.Permissions.SecurityAction> 枚举，以及要向其授予权限的文件或目录的位置。 **Assert** `C:\Log.txt` 即使不检查调用方是否有权访问该文件，调用断言也会导致访问权限成功。  
  
```vb  
Option Explicit  
Option Strict  
  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
  
      End Sub  
  
     <FileIOPermission(SecurityAction.Assert, All := "C:\Log.txt")> Public Sub
      MakeLog()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now) '  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {
      }
      [FileIOPermission(SecurityAction.Assert, All = @"C:\Log.txt")]  
      public void MakeLog()  
      {
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}
```  
  
 下面的代码片段演示了使用**Assert**方法重写安全检查的命令性语法。 在此示例中，声明了**FileIOPermission**对象的实例。 其构造函数被传递**FileIOPermissionAccess**以定义允许的访问类型，后跟描述文件位置的字符串。 定义**FileIOPermission**对象后，只需调用其**Assert**方法来重写安全检查。  
  
```vb  
Option Explicit  
Option Strict  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
      End Sub 'New  
  
      Public Sub MakeLog()  
         Dim FilePermission As New FileIOPermission(FileIOPermissionAccess.AllAccess, "C:\Log.txt")  
         FilePermission.Assert()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now)  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {
      }
      public void MakeLog()  
      {  
         FileIOPermission FilePermission = new FileIOPermission(FileIOPermissionAccess.AllAccess,@"C:\Log.txt");
         FilePermission.Assert();  
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Security.PermissionSet>
- <xref:System.Security.Permissions.SecurityPermission>
- <xref:System.Security.Permissions.FileIOPermission>
- <xref:System.Security.Permissions.SecurityAction>
- [特性](../../standard/attributes/index.md)
- [代码访问安全性](code-access-security.md)
