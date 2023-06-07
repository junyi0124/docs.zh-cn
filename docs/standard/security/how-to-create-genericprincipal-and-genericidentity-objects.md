---
title: 如何：创建 GenericPrincipal 和 GenericIdentity 对象
ms.date: 07/15/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Creating Generic Identity Objects
- GenericPrincipal Objects
- Creating GenericPrincipal Objects
- GenericIdentity Objects
ms.assetid: 465694cf-258b-4747-9dae-35b01a5bcdbb
ms.openlocfilehash: cde4669a1bac49d1d9fde39c99707561379aec19
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818988"
---
# <a name="how-to-create-genericprincipal-and-genericidentity-objects"></a>如何：创建 GenericPrincipal 和 GenericIdentity 对象

> [!NOTE]
> 本文适用于 Windows。
>
> 有关 ASP.NET Core 的信息，请参阅 [ASP.NET Core 安全性概述](/aspnet/core/security/)。

你可以将 <xref:System.Security.Principal.GenericIdentity> 类与类结合使用 <xref:System.Security.Principal.GenericPrincipal> ，以创建独立于 Windows 域的授权方案。

### <a name="to-create-a-genericprincipal-object"></a>创建 GenericPrincipal 对象

1. 创建标识类的一个新实例，并用希望它持有的名称对其进行初始化。 以下代码创建一个新的 **GenericIdentity** 对象，并用名称 `MyUser` 对其进行初始化。

    ```vb
    Dim myIdentity As New GenericIdentity("MyUser")
    ```

    ```csharp
    GenericIdentity myIdentity = new GenericIdentity("MyUser");
    ```

2. 创建 **GenericPrincipal** 类的一个新实例，并用先前创建的 **GenericIdentity** 对象和表示希望与此主体关联的角色的字符串数组对其进行初始化。 下面的代码示例指定表示一个管理员角色和一个用户角色的字符串数组。 然后用前面的 **GenericIdentity** 和该字符串数组对 **GenericPrincipal** 进行初始化。

    ```vb
    Dim myStringArray As String() = {"Manager", "Teller"}
    DIm myPrincipal As New GenericPrincipal(myIdentity, myStringArray)
    ```

    ```csharp
    String[] myStringArray = {"Manager", "Teller"};
    GenericPrincipal myPrincipal = new GenericPrincipal(myIdentity, myStringArray);
    ```

3. 使用以下代码将主体附加到当前线程中。 这在以下情况下很有用：必须多次验证主体，必须由应用程序中运行的其他代码验证，或者必须由 <xref:System.Security.Permissions.PrincipalPermission> 对象验证。 不将主体附加到线程中，仍可对主体对象执行基于角色的验证。 有关详细信息，请参阅[替换主体对象](replacing-a-principal-object.md)。

    ```vb
    Thread.CurrentPrincipal = myPrincipal
    ```

    ```csharp
    Thread.CurrentPrincipal = myPrincipal;
    ```

## <a name="example"></a>示例

下面的代码示例说明如何创建 **GenericPrincipal** 和 **GenericIdentity** 的实例。 此代码将这些对象的值显示到控制台中。

```vb
Imports System.Security.Principal
Imports System.Threading

Public Class Class1

    Public Shared Sub Main()
        ' Create generic identity.
        Dim myIdentity As New GenericIdentity("MyIdentity")

        ' Create generic principal.
        Dim myStringArray As String() =  {"Manager", "Teller"}
        Dim myPrincipal As New GenericPrincipal(myIdentity, myStringArray)

        ' Attach the principal to the current thread.
        ' This is not required unless repeated validation must occur,
        ' other code in your application must validate, or the
        ' PrincipalPermission object is used.
        Thread.CurrentPrincipal = myPrincipal

        ' Print values to the console.
        Dim name As String = myPrincipal.Identity.Name
        Dim auth As Boolean = myPrincipal.Identity.IsAuthenticated
        Dim isInRole As Boolean = myPrincipal.IsInRole("Manager")

        Console.WriteLine("The name is: {0}", name)
        Console.WriteLine("The isAuthenticated is: {0}", auth)
        Console.WriteLine("Is this a Manager? {0}", isInRole)

    End Sub

End Class
```

```csharp
using System;
using System.Security.Principal;
using System.Threading;

public class Class1
{
    public static int Main(string[] args)
    {
    // Create generic identity.
    GenericIdentity myIdentity = new GenericIdentity("MyIdentity");

    // Create generic principal.
    String[] myStringArray = {"Manager", "Teller"};
    GenericPrincipal myPrincipal =
        new GenericPrincipal(myIdentity, myStringArray);

    // Attach the principal to the current thread.
    // This is not required unless repeated validation must occur,
    // other code in your application must validate, or the
    // PrincipalPermission object is used.
    Thread.CurrentPrincipal = myPrincipal;

    // Print values to the console.
    String name =  myPrincipal.Identity.Name;
    bool auth =  myPrincipal.Identity.IsAuthenticated;
    bool isInRole =  myPrincipal.IsInRole("Manager");

    Console.WriteLine("The name is: {0}", name);
    Console.WriteLine("The isAuthenticated is: {0}", auth);
    Console.WriteLine("Is this a Manager? {0}", isInRole);

    return 0;
    }
}
```

执行时，应用程序显示类似于下面这样的输出。

```console
The Name is: MyIdentity
The IsAuthenticated is: True
Is this a Manager? True
```

## <a name="see-also"></a>另请参阅

- <xref:System.Security.Principal.GenericIdentity>
- <xref:System.Security.Principal.GenericPrincipal>
- <xref:System.Security.Permissions.PrincipalPermission>
- [替换 Principal 对象](replacing-a-principal-object.md)
- [主体和标识对象](principal-and-identity-objects.md)
