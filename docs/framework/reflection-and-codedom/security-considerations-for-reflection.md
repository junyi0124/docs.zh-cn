---
title: 反射的安全注意事项
description: 了解 .NET 中的反射的安全注意事项。 允许获取有关类型和成员的信息，但访问成员有限制。
ms.date: 03/30/2017
helpviewer_keywords:
- permissions [.NET Framework], reflection
- MethodInfo parameters
- reflection, security
- partial trust,reflection
- nonpublic members
- reflection,partial trust
- link demands
ms.assetid: 42d9dc2a-8fcc-4ff3-b002-4ff260ef3dc5
ms.openlocfilehash: 9ef2ac4897b3f8c48a0b0f402ab06eb073a5c1fa
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556331"
---
# <a name="security-considerations-for-reflection"></a>反射的安全注意事项

通过反射能够获取有关类型和成员的信息，并能访问成员（即，调用方法和构造函数来获取和设置属性值，添加和移除事件处理程序，等等）。 使用反射可以获取有关类型的信息并且成员是不受限制的。 所有代码都可使用反射来执行以下任务：

- 枚举类型和成员，并检查其元数据。

- 枚举并检查程序集和模块。

与之相反，使用反射来访问成员会受到限制。 从 .NET Framework 4 开始，只有受信任的代码才能使用反射来访问安全关键成员。 而且，只有受信任的代码才能使用反射访问无法由已编译代码直接访问的非公共成员。 最后，使用反射访问关键安全成员的代码必须具有关键安全成员要求的任何权限，就像编译的代码一样。

具有一定的权限，代码可以使用反射来执行以下类型的访问：

- 访问不是安全关键的公共成员。

- 若这些成员不是安全关键，则访问可进入编译代码的非公共成员。 此类非公共成员的示例包括：

  - 调用代码的基础类的受保护成员。 （在反射中，这称为系列级访问权限。）

  - 调用代码的程序集中的 `internal` 成员（Visual Basic 中的 `Friend` 成员）。 （在反射中，这称为程序集级别的访问。）

  - 包含调用代码的类的其他实例的私有成员。

例如，在沙盒应用程序域中运行的代码被限制于此列表所述的访问权限，除非该应用程序域授予其他权限。

从 .NET Framework 2.0 Service Pack 1 开始，尝试访问通常无法访问的成员将生成目标对象授权集的需求以及带 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。 以“完全信任”运行的代码（比如，从命令行启动的应用程序中的代码）始终可以满足这些权限。 （如本文后续部分所述，访问安全关键成员时会受到限制。）

沙盒应用程序域可以向 <xref:System.Security.Permissions.ReflectionPermission> 授予 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志，如本文后续部分中的[访问通常不可访问的成员](#accessingNormallyInaccessible)中所述。

<a name="accessingSecurityCritical"></a>

## <a name="accessing-security-critical-members"></a>访问安全关键成员

一个成员如果具有 <xref:System.Security.SecurityCriticalAttribute>，而它属于具有 <xref:System.Security.SecurityCriticalAttribute> 的类型，或是它在安全关键程序集中，则该成员为安全关键成员。 从 .NET Framework 4 开始，访问安全关键成员的规则如下：

- 透明代码不能使用反射来访问安全关键成员，即使是完全受信任的代码。 引发一个 <xref:System.MethodAccessException>、<xref:System.FieldAccessException> 或 <xref:System.TypeAccessException>。

- 使用部分信任运行的代码将被视为透明。

无论是通过已编译代码直接访问还是使用反射访问安全关键成员，这些规则都不会变。

从命令行运行的应用程序代码将以“完全信任”运行。 只要不被标记为透明，它就可以使用反射来访问安全关键成员。 当同一代码以部分信任运行时（例如，在沙箱应用程序域中），程序集的信任级别将决定其是否能够访问安全关键代码：如果程序集有强名称并安装在全局程序集缓存中，则是受信任的程序集，可以调用安全关键成员。 如果不是受信任的，即使未标记为透明，它也将变为透明，并且它不能访问安全关键成员。

有关 .NET Framework 4 中安全模型的详细信息，请参阅[安全更改](/previous-versions/dotnet/framework/security/security-changes)。

## <a name="reflection-and-transparency"></a>反射和透明度

以 .NET Framework 4 开始，公共语言运行时从若干方面确定一个类型或成员的透明度级别，包括程序集和应用程序域的信任级别。 反射提供了 <xref:System.Type.IsSecurityCritical%2A>、<xref:System.Type.IsSecuritySafeCritical%2A> 和 <xref:System.Type.IsSecurityTransparent%2A> 属性，以使你能够发现类型的透明度级别。 下表显示了这些属性的有效组合。

|安全级别|IsSecurityCritical|IsSecurityCritical|IsSecurityTransparent|
|--------------------|------------------------|----------------------------|---------------------------|
|严重|`true`|`false`|`false`|
|安全-关键|`true`|`true`|`false`|
|透明|`false`|`false`|`true`|

使用这些属性比检查程序集及其类型的安全批注、检查当前的信任级别，以及尝试复制运行时的规则要简单得多。 例如，当从命令行中运行时，相同的类型可以是安全关键，或者在沙盒应用程序域中运行时，它们又是安全-透明的。

在 <xref:System.Reflection.MethodBase>、<xref:System.Reflection.FieldInfo>、<xref:System.Reflection.Emit.TypeBuilder>、<xref:System.Reflection.Emit.MethodBuilder> 和 <xref:System.Reflection.Emit.DynamicMethod> 上有类似的属性。 （对于其他反射和反射发出抽象化，安全属性应用到关联的方法；例如，在它们应用于属性访问器的属性的情况下）。

<a name="accessingNormallyInaccessible"></a>

## <a name="accessing-members-that-are-normally-inaccessible"></a>访问通常不可访问的成员

根据公共语言运行时的可访问性规则，若要使用反射来调用无法访问的成员，你的代码必须获得以下两个权限之一：

- 若要允许代码调用任何非公共成员：代码必须获得带 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。

  > [!NOTE]
  > 默认情况下，安全策略拒绝源于 Internet 的代码的权限。 此权限永远不会授权予源自 Internet 的代码。

- 要允许代码调用任何非公共成员，只要包含调用成员的程序集的授予集与包含调用代码的程序集的授予集相同或与其子集相同：你的代码必须授予带 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。

例如，假设你为应用程序域授予 Internet 权限以及带 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>，则使用两个程序集 A 和 B 运行 Internet 应用程序。

- 程序集 A 可以使用反射来访问程序集 B 的私有成员，因为程序集 B 的授予集不包括一个 A 尚未被授予的任何权限。

- 程序集 A 不能使用反射来访问 .NET Framework 程序集的私有成员（如 mscorlib.dll），因为 mscorlib.dll 是完全受信任的，因此有尚未被授予给程序集 A 的权限。代码访问安全性在运行时审核堆栈将引发 <xref:System.MemberAccessException>。

## <a name="serialization"></a>序列化

对于序列化，带 <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter%2A?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.SecurityPermission>，无论其访问级别是什么，都能够获取和设置序列化类型的成员。 此权限使代码可以发现并更改实例的私有状态。 （除被授予适当权限以外，在元数据中该类型必须[标记](../../standard/attributes/applying-attributes.md)为可序列化。）

## <a name="parameters-of-type-methodinfo"></a>类型 MethodInfo 的参数

尤其是对受信任的代码，要避免编写采用 <xref:System.Reflection.MethodInfo> 参数的公共成员。 此类成员可能更容易受到恶意代码的攻击。 例如，考虑采用 <xref:System.Reflection.MethodInfo> 参数的高度受信任代码中的公共成员。 假定公共成员间接调用所提供参数 <xref:System.Reflection.MethodBase.Invoke%2A> 上的方法。 如果公共成员不执行必要的权限检查，对 <xref:System.Reflection.MethodBase.Invoke%2A> 方法的调用会一直成功，因为安全系统确定了该调用方为高度受信任的调用方。 即使恶意代码没有直接调用该方法的权限，它仍可以通过调用公共成员间接调用该方法。

## <a name="version-information"></a>版本信息

- 从 .NET Framework 4 开始，透明代码不能使用反射来访问安全关键成员。

- .NET Framework 2.0 Service Pack 1 中引入了 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志。 早期版本的 .NET Framework 需要使用反射访问非公共成员的代码的 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志。 这是绝对不会授予给部分受信任的代码的权限。

- 从 .NET Framework 2.0 开始，使用反射获取关于非公共类型和成员的信息不需要任何权限。 早期版本中，需要带 <xref:System.Security.Permissions.ReflectionPermissionFlag.TypeInformation?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。

## <a name="see-also"></a>请参阅

- <xref:System.Security.Permissions.ReflectionPermissionFlag>
- <xref:System.Security.Permissions.ReflectionPermission>
- <xref:System.Security.Permissions.SecurityPermission>
- [安全更改](/previous-versions/dotnet/framework/security/security-changes)
- [代码访问安全性](../misc/code-access-security.md)
- [反射发出中的安全问题](security-issues-in-reflection-emit.md)
- [查看类型信息](viewing-type-information.md)
- [应用特性](../../standard/attributes/applying-attributes.md)
- [访问自定义属性](accessing-custom-attributes.md)
