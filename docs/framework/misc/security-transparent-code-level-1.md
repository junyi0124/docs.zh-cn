---
title: 安全透明的代码，级别 1
description: 查看1级透明度代码模型、透明度属性和安全透明度示例。
ms.date: 03/30/2017
helpviewer_keywords:
- transparent
- SecurityTreatAsSafeAttribute
- SecurityTransparentAttribute
- SecurityCriticalAttribute
- security-transparent code
- security [.NET Framework], security-transparent code
ms.assetid: 5fd8f46d-3961-46a7-84af-2eb1f48e75cf
ms.openlocfilehash: 97acccdc1dcab11e42d116f4743e1182029e2dd6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288185"
---
# <a name="security-transparent-code-level-1"></a>安全透明的代码，级别 1

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 透明度可帮助开发人员编写更安全的 .NET Framework 库来向部分信任的代码公开功能。 .NET Framework 2.0 版中引入了 1 级透明度，此透明度主要仅供 Microsoft 内部使用。 从 .NET Framework 4 开始，可以使用 [2 级透明度](security-transparent-code-level-2.md)。 但已保留 1 级透明度，以便你能够标识必须使用以前的安全规则运行的旧代码。  
  
> [!IMPORTANT]
> 应仅出于兼容性目的而指定 1 级透明度；也就是说，仅为使用 .NET Framework 3.5 或以前的版本（使用 <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 或不使用透明度模型）开发的代码指定 1 级透明度。 例如，对允许从部分信任的调用方 (APTCA) 调用的 .NET Framework 2.0 程序集使用 1 级透明度。 对于为 .NET Framework 4 开发的代码，请始终使用2级透明度。  
  
 本主题包含以下各节：  
  
- [1 级透明度模型](#the_level_1_transparency_model)  
  
- [透明度特性](#transparency_attributes)  
  
- [安全透明度示例](#security_transparency_examples)  
  
<a name="the_level_1_transparency_model"></a>

## <a name="the-level-1-transparency-model"></a>1 级透明度模型  

 使用 1 级透明度时，你会使用一种安全模型，该模型将代码分成三类方法：安全透明的方法、安全可靠关键的方法和安全关键的方法。  
  
 可将整个程序集、程序集中的某些类或类中的某些方法标记为安全透明。 安全透明的代码不能提升特权。 此限制会造成三个后果：  
  
- 安全透明的代码不能执行 <xref:System.Security.Permissions.SecurityAction.Assert> 操作。  
  
- 安全透明的代码将满足的任何链接要求都会变成完全要求。  
  
- 若有任何必须在安全透明的代码中执行的不安全代码（不可验证的代码），都会引起对 <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode> 安全权限的完全要求。  
  
 在执行期间，公共语言运行时 (CLR) 会强制执行这些规则。 安全透明的代码会将其调用的代码的所有安全需求全部传递回调用方。 流过安全透明代码的要求不能提升特权。 如果低信任应用程序调用安全透明的代码并导致对高特权的要求，该要求将流回低信任代码并失败。 由于安全透明的代码不能执行断言操作，因此该代码不能停止该要求。 从完全信任代码中调用相同的安全透明代码将使要求成功。  
  
 安全关键与安全透明相反。 安全关键代码以完全信任模式执行，可以执行所有特权操作。 安全可靠关键代码是特权代码，并经过全面的安全审核，已确认它不允许部分信任的调用方使用它们无权访问的资源。  
  
 必须显式应用透明度。 通常，处理数据操作和逻辑的大部分代码都可以标记为安全透明，而执行特权提升的少量代码则标记为安全关键或安全可靠关键。  
  
> [!IMPORTANT]
> 1 级透明度限于程序集范围，不能跨程序集实施。 1 级透明度主要在 Microsoft 内部出于安全审核的目的使用。 1 级程序集中的安全关键类型和成员可由其他程序集中的安全透明代码访问。 在所有 1 级安全关键类型和成员中，都必须执行针对完全信任的链接要求。 对于安全可靠关键类型或成员所访问的受保护资源，该类型和成员还必须确认调用方拥有访问这些资源的权限。  
  
 为了向后兼容 .NET Framework 的早期版本，所有未用透明度特性批注的成员都被视为安全可靠关键成员。 所有未进行此类批注的类型都被视为是透明的。 没有用于验证透明度的静态分析规则。 因此，你可能需要在运行时调试透明度错误。  
  
<a name="transparency_attributes"></a>

## <a name="transparency-attributes"></a>透明度特性  

 下表介绍可用于批注代码透明度的三个特性。  
  
|Attribute|描述|  
|---------------|-----------------|  
|<xref:System.Security.SecurityTransparentAttribute>|只允许在程序集级别上应用。 将程序集中的所有类型和成员都标识为安全透明。 程序集不能包含任何安全关键代码。|  
|<xref:System.Security.SecurityCriticalAttribute>|不带 <xref:System.Security.SecurityCriticalAttribute.Scope%2A> 属性在程序集级别使用时，默认将程序集中的所有代码都标识为安全透明的代码，但会指出程序集可能包含安全关键代码。<br /><br /> 在类级别使用时，将类或方法标识为安全关键，但不会如此标识类的成员。 若要使所有成员都成为安全关键成员，请将 <xref:System.Security.SecurityCriticalAttribute.Scope%2A> 属性设置为 <xref:System.Security.SecurityCriticalScope.Everything>。<br /><br /> 在成员级别使用时，特性仅应用于相应的成员。<br /><br /> 标识为安全关键的类或成员可以执行特权提升。 **重要提示：**  在第1级透明度中，当从程序集外部调用安全关键类型和成员时，这些类型和成员将被视为安全可靠关键类型。 应该通过针对完全信任的链接要求来保护安全关键类型和成员，以避免未经授权的特权提升。|  
|<xref:System.Security.SecuritySafeCriticalAttribute>|标识可以由程序集中的安全透明代码访问的安全关键代码。 否则，安全透明的代码将不能访问同一程序集中的私有或内部安全关键成员。 执行此操作将影响安全关键代码，并且可能会引起意外的特权提升。 安全可靠关键代码应该经过严格的安全审核。 **注意：**  安全可靠关键类型和成员必须验证调用方的权限，以确定调用方是否有权访问受保护的资源。|  
  
 <xref:System.Security.SecuritySafeCriticalAttribute> 特性使得安全透明的代码可以访问同一程序集中的安全关键成员。 将程序集中的安全透明代码和安全关键代码视为两个程序集中的代码。 安全透明的代码将不能查看安全关键代码的私有或内部成员。 此外，通常会对安全关键代码进行审核，以核查对其公共接口的访问权限。 你不希望可在程序集外访问私有或内部状态，而希望将该状态保持隔离。 <xref:System.Security.SecuritySafeCriticalAttribute> 特性可在安全透明代码和安全关键代码之间保持状态隔离，同时允许在必要时替代该隔离。 安全透明的代码不能访问私有或内部安全关键代码，除非这些成员已标记有 <xref:System.Security.SecuritySafeCriticalAttribute>. 在应用 <xref:System.Security.SecuritySafeCriticalAttribute> 之前，将该成员视为公共公开的成员对其进行审核。  
  
### <a name="assembly-wide-annotation"></a>程序集范围的批注  

 下表描述在程序集级别使用安全特性所产生的作用。  
  
|程序集属性|程序集状态|  
|------------------------|--------------------|  
|部分信任的程序集上无特性|所有类型和成员都是透明的。|  
|完全信任的程序集上无特性（对于全局程序集缓存中的程序集，或者在 `AppDomain` 中标识为完全信任的程序集）|所有类型都是透明的，所有成员都是安全可靠关键的。|  
|`SecurityTransparent`|所有类型和成员都是透明的。|  
|`SecurityCritical(SecurityCriticalScope.Everything)`|所有类型和成员都是安全关键的。|  
|`SecurityCritical`|所有代码默认都是透明的。 但是，各个类型和成员可以有其他特性。|  
  
<a name="security_transparency_examples"></a>

## <a name="security-transparency-examples"></a>安全透明度示例  

 若要使用 .NET Framework 2.0 透明度规则（1 级透明度），请使用下面的程序集批注：  
  
```csharp
[assembly: SecurityRules(SecurityRuleSet.Level1)]  
```  
  
 如果要使整个程序集透明以指示该程序集不包含任何关键代码并且不以任何方式提升特权，则可通过以下特性显式增加程序集的透明度：  
  
```csharp  
[assembly: SecurityTransparent]  
```  
  
 如果要在同一程序集中混合关键代码和透明代码，则可首先用 <xref:System.Security.SecurityCriticalAttribute> 特性标记该程序集，以指示该程序集可以包含关键代码，如下所示：  
  
```csharp  
[assembly: SecurityCritical]  
```  
  
 如果要执行安全关键操作，则必须用另一个 <xref:System.Security.SecurityCriticalAttribute> 特性显式标记将执行关键操作的代码，如下面的代码示例中所示：  
  
```csharp  
[assembly: SecurityCritical]  
public class A  
{  
    [SecurityCritical]  
    private void Critical()  
    {  
        // critical  
    }  
  
    public int SomeProperty  
    {  
        get {/* transparent */ }  
        set {/* transparent */ }  
    }  
}  
public class B  
{
    internal string SomeOtherProperty  
    {  
        get { /* transparent */ }  
        set { /* transparent */ }  
    }  
}  
```  
  
 除 `Critical` 方法显式标记为安全关键之外，前面的代码都是透明的。 即使使用程序集级别的 <xref:System.Security.SecurityCriticalAttribute> 特性，透明度也是默认设置。  
  
## <a name="see-also"></a>另请参阅

- [安全透明的代码，级别 2](security-transparent-code-level-2.md)
- [安全更改](/previous-versions/dotnet/framework/security/security-changes)
