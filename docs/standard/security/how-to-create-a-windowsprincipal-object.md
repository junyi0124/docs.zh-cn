---
title: 如何：创建 WindowsPrincipal 对象
ms.date: 07/15/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WindowsPrincipal objects, creating
- security [.NET], creating a WindowsPrincipal object
- security [.NET], principals
- principal objects, creating
ms.assetid: 56eb10ca-e61d-4ed2-af7a-555fc4c25a25
ms.openlocfilehash: d4140470308a09774e5e4eee429c1e91b559d063
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819027"
---
# <a name="how-to-create-a-windowsprincipal-object"></a>如何：创建 WindowsPrincipal 对象

> [!NOTE]
> 本文适用于 Windows。
>
> 有关 ASP.NET Core 的信息，请参阅 [ASP.NET Core 安全性](/aspnet/core/security/)。

有两种方法来创建 <xref:System.Security.Principal.WindowsPrincipal> 对象，具体取决于代码必须重复执行基于角色的验证还是必须只能执行一次。  
  
如果代码必须重复执行基于角色的验证，则下列过程的第一个将生成更少的开销。 当代码只需要进行一次基于角色的验证时，你可以使用以下过程的第二个来创建 <xref:System.Security.Principal.WindowsPrincipal> 对象。  
  
### <a name="to-create-a-windowsprincipal-object-for-repeated-validation"></a>若要创建 WindowsPrincipal 对象用于重复验证  
  
1. 在由静态 <xref:System.AppDomain.CurrentDomain%2A?displayProperty=nameWithType> 属性所返回的 <xref:System.AppDomain> 对象上调用 <xref:System.AppDomain.SetPrincipalPolicy%2A>方法，向此方法传递 <xref:System.Security.Principal.PrincipalPolicy>枚举值，该值指示新策略应该是什么。 支持的值为 <xref:System.Security.Principal.PrincipalPolicy.NoPrincipal>、<xref:System.Security.Principal.PrincipalPolicy.UnauthenticatedPrincipal> 和 <xref:System.Security.Principal.PrincipalPolicy.WindowsPrincipal>。 下面的代码演示了此方法调用。  
  
    ```csharp  
    AppDomain.CurrentDomain.SetPrincipalPolicy(  
        PrincipalPolicy.WindowsPrincipal);  
    ```  
  
    ```vb  
    AppDomain.CurrentDomain.SetPrincipalPolicy( _  
        PrincipalPolicy.WindowsPrincipal)  
    ```  
  
2. 通过策略设置，使用静态 <xref:System.Threading.Thread.CurrentPrincipal%2A?displayProperty=nameWithType> 属性来检索封装当前 Windows 用户的主体。 由于属性返回类型是 <xref:System.Security.Principal.IPrincipal>，因此你必须将结果转换为 <xref:System.Security.Principal.WindowsPrincipal> 类型。 以下代码将一个新 <xref:System.Security.Principal.WindowsPrincipal> 对象初始化为与当前线程关联的主体的值。  
  
    ```csharp  
    WindowsPrincipal myPrincipal =
        (WindowsPrincipal) Thread.CurrentPrincipal;  
    ```  
  
    ```vb  
    Dim myPrincipal As WindowsPrincipal = _  
        CType(Thread.CurrentPrincipal, WindowsPrincipal)
    ```  
  
3. 创建主体对象后，你可以使用若干方法中的一种对其进行验证。  
  
### <a name="to-create-a-windowsprincipal-object-for-a-single-validation"></a>若要创建 WindowsPrincipal 对象用于单个验证  
  
1. 通过调用静态 <xref:System.Security.Principal.WindowsIdentity.GetCurrent%2A?displayProperty=nameWithType> 方法初始化新的 <xref:System.Security.Principal.WindowsIdentity> 对象，该方法查询当前的 Windows 帐户并将有关该帐户的信息放置到新创建的标识对象中。 下面的代码创建一个新的 <xref:System.Security.Principal.WindowsIdentity> 对象并将其初始化为当前经过身份验证的用户。  
  
    ```csharp  
    WindowsIdentity myIdentity = WindowsIdentity.GetCurrent();  
    ```  
  
    ```vb  
    Dim myIdentity As WindowsIdentity = WindowsIdentity.GetCurrent()  
    ```  
  
2. 创建一个新的 <xref:System.Security.Principal.WindowsPrincipal> 对象并向其传递在上一步中创建的 <xref:System.Security.Principal.WindowsIdentity> 对象的值。  
  
    ```csharp  
    WindowsPrincipal myPrincipal = new WindowsPrincipal(myIdentity);  
    ```  
  
    ```vb  
    Dim myPrincipal As New WindowsPrincipal(myIdentity)  
    ```  
  
3. 创建主体对象后，你可以使用若干方法中的一种对其进行验证。  
  
## <a name="see-also"></a>另请参阅

- [主体和标识对象](principal-and-identity-objects.md)
- [ASP.NET Core 安全性](/aspnet/core/security/)
