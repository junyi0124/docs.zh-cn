---
title: 保护方法访问
description: 了解如何对不打算公开使用但仍需要公共的方法进行方法访问。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- code security, method access
- secure coding, method access
- security [.NET Framework], method access
- method access security
ms.assetid: f7c2d6ec-3b18-4e0e-9991-acd97189d818
ms.openlocfilehash: 52ae1eb4b6210403ce9c5aa96479809f885b0eba
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251212"
---
# <a name="securing-method-access"></a>保护方法访问

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 某些方法可能不适用于允许任意不受信任的代码进行调用。 此类方法会带来几种风险：方法可能会提供一些受限制的信息；它可能信任传递给它的任何信息；它可能不会对参数执行错误检查；如果使用错误的参数，它还可能出现故障或执行某些有害操作。 应该注意这些情况，并采取措施以帮助保护这类方法。  
  
 在某些情况下，可能需要限制并不打算用于公共用途但仍须为公共方法的方法。 例如，可能有一个需在你自己的 DLL 间调用（从而必须为公共接口）的接口，但你不希望将其以公共方式公开，以防止客户使用它，或防止恶意代码利用入口点进入组件中。 限制不用于公共使用的方法的另一个常见原因是 (但必须是公共的) ，以避免必须记录和支持可能是内部接口的内容。  
  
 托管代码提供几种方法来限制方法访问：  
  
- 如果可以信任类、程序集或派生类，则限制它们的可访问性的范围。 这是限制方法访问的最简单的方法。 通常，派生类的可信度可能低于它们派生自的类，但在某些情况下，它们共享父类的标识。 特别是，不要从关键字推断信任 `protected` ，这并不一定在安全上下文中使用。  
  
- 限制对指定标识的调用方的方法访问-实质上，任何特定 [证据](/previous-versions/dotnet/netframework-4.0/7y5x1hcd(v=vs.100)) (强名称、发布服务器、区域等) 你选择。  
  
- 限制对具有你选择的任何权限的调用方的方法访问。  
  
 同样，声明性安全使你可以控制类的继承。 您可以使用 **InheritanceDemand** 来执行以下操作：  
  
- 要求派生类具有指定的标识或权限。  
  
- 要求重写特定方法的派生类具有指定的标识或权限。  
  
 下面的示例演示如何通过要求使用特定强名称对调用方进行签名来帮助保护具有有限访问权限的公共类。 此示例使用 <xref:System.Security.Permissions.StrongNameIdentityPermissionAttribute> 带有强名称 **需求** 的。 有关如何使用强名称为程序集签名的信息，请参阅 [创建和使用 Strong-Named 程序集](../../standard/assembly/create-use-strong-named.md)。  
  
```vb  
<StrongNameIdentityPermissionAttribute(SecurityAction.Demand, PublicKey := "…hex…", Name := "App1", Version := "0.0.0.0")>  _  
Public Class Class1  
End Class  
```  
  
```csharp  
[StrongNameIdentityPermissionAttribute(SecurityAction.Demand, PublicKey="…hex…", Name="App1", Version="0.0.0.0")]  
public class Class1  
{  
  
}
```  
  
## <a name="excluding-classes-and-members-from-use-by-untrusted-code"></a>防止不受信任的代码使用类和成员  

 使用本节中所示的声明来防止部分受信任的代码使用特定的类、方法以及属性和事件。 通过将这些声明应用于类，可以将保护应用于其所有方法、属性和事件。 但是，字段访问不受声明性安全的影响。 也请注意，链接要求仅帮助不受直接调用方的攻击，可能仍会受到引诱攻击。  
  
> [!NOTE]
> .NET Framework 4 中引入了一个新的透明度模型。 [安全透明的代码，级别 2](security-transparent-code-level-2.md)模型用属性标识安全代码 <xref:System.Security.SecurityCriticalAttribute> 。 安全关键代码需要调用方和继承者均完全受信任。 在早期 .NET Framework 版本中的代码访问安全性规则下运行的程序集可以调用级别 2 程序集。 在这种情况下，安全关键属性将被视为完全信任的链接要求。  
  
 在强名称程序集中， [LinkDemand](link-demands.md) 应用于所有可公开访问的方法、属性和事件，以将其使用限制为完全受信任的调用方。 若要禁用此功能，必须应用 <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 特性。 因此，仅未签名的程序集或具有此特性的程序集需要显式标记类以排除不受信任调用方；可以使用这些声明来标记其中并不打算用于不受信任的调用方的类型子集。  
  
 下面的示例说明如何防止不受信任的代码使用类和成员。  
  
 对于公共非密封类：  
  
```vb  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name := "FullTrust"), _
System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name := "FullTrust")>  _  
Public Class CanDeriveFromMe  
End Class  
```  
  
```csharp  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name="FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
public class CanDeriveFromMe  
{  
}  
```  
  
 对于公共密封类：  
  
```vb  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name := "FullTrust")>  _  
NotInheritable Public Class CannotDeriveFromMe  
End Class  
```  
  
```csharp  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
public sealed class CannotDeriveFromMe  
{  
}  
```  
  
 对于公共抽象类：  
  
```vb  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name := "FullTrust"), _  
System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name := "FullTrust")>  _  
MustInherit Public Class CannotCreateInstanceOfMe_CanCastToMe  
End Class  
```  
  
```csharp  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name="FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
public abstract class CannotCreateInstanceOfMe_CanCastToMe {}  
```  
  
 对于公共虚拟函数：  
  
```vb  
Class Base1
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")> _  
    Public Overridable Sub CanOverrideOrCallMe()  
    End Sub 'CanOverrideOrCallMe  
End Class 'Base1  
```  
  
```csharp  
class Base1
{  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.InheritanceDemand, Name="FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
    public virtual void CanOverrideOrCallMe() {}  
}  
```  
  
 对于公共抽象函数：  
  
```vb  
MustInherit Class Base2  
    <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")> _  
    Public Sub MustOverrideMe()  
    End Sub  
End Class 'Base2  
```  
  
```csharp  
abstract class Base2 {  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.InheritanceDemand, Name = "FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]  
public abstract void MustOverrideMe();  
}  
```  
  
 对于基类不要求完全信任的公共重写函数：  
  
```vb  
Class Derived  
    Inherits Base1  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.Demand, Name:="FullTrust")> _  
    Public Overrides Sub CanOverrideOrCallMe()  
        MyBase.CanOverrideOrCallMe()  
    End Sub 'CanOverrideOrCallMe  
End Class 'Derived  
```  
  
```csharp  
class Derived : Base1  
{
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.Demand, Name="FullTrust")]
    public override void CanOverrideOrCallMe()
    {  
        base.CanOverrideOrCallMe();  
    }  
}  
```  
  
 对于基类要求完全信任的公共重写函数：  
  
```vb  
Class Derived  
    Inherits Base1  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")> _  
    Public Overrides Sub CanOverrideOrCallMe()  
        MyBase.CanOverrideOrCallMe()  
    End Sub 'CanOverrideOrCallMe
End Class 'Derived  
```  
  
```csharp  
class Derived : Base1  
{
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]
    public override void CanOverrideOrCallMe()
    {  
        base.CanOverrideOrCallMe();  
    }  
}  
```  
  
 对于公共接口：  
  
```vb  
Public Interface ICanCastToMe  
    <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust")> _  
    Sub CanImplementMe()  
End Interface 'ICanCastToMe  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust")> _  
Class Implemented  
    Implements ICanCastToMe  
    Public Sub CanImplementMe()  
    End Sub 'CanImplementMe  
```  
  
```csharp  
public interface ICanCastToMe
{  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name = "FullTrust")]  
void CanImplementMe();  
}  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name = "FullTrust")]  
class Implemented : ICanCastToMe  
{  
    public void CanImplementMe()  
    {  
    }  
}  
```  
  
## <a name="virtual-internal-overrides-or-overloads-overridable-friend"></a>Virtual Internal 重写或 Overloads Overridable Friend  
  
> [!NOTE]
> 本部分对在 `virtual` `internal` `Overloads` `Overridable` `Friend` Visual Basic) 中将方法声明为和 (时的安全问题进行了警告。 此警告仅适用于 .NET Framework 版本1.0 和1.1，不适用于更高版本。  
  
 在 .NET Framework 版本1.0 和1.1 中，你必须了解类型系统可访问性在确认你的代码对其他程序集不可用时的细微差别。 在 Visual Basic) 可以重写父类项的情况中，声明为 **虚拟** 和 **内部** (重载可重写 **Friend** 的方法，并且只能在同一程序集中使用该方法，因为它是内部的。 但是，重写的可访问性由 **virtual** 关键字确定，只要代码有权访问类本身，就可以从其他程序集进行重写。 如果重写的可能性导致了问题，请使用声明性安全修复此问题，如果不是绝对必需的，则删除 **虚拟** 关键字。  
  
 即使语言编译器阻止这些重写的编译错误，使用其他编译器编写的代码也可能会重写。  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](../../standard/security/secure-coding-guidelines.md)
