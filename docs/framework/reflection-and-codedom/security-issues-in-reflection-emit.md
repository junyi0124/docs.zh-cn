---
title: 反射发出中的安全问题
description: 了解反射发出中的安全问题，反射发出是通过动态程序集或者与现有程序集关联的动态方法或匿名托管的动态方法完成的。
ms.date: 03/30/2017
helpviewer_keywords:
- partially trusted code
- emitting dynamic assemblies, security
- reflection emit, security
- reflection emit, partial trust scenarios
- partial trust,emitting dynamic methods
- anonymously hosted dynamic methods [.NET Framework]
- emitting dynamic assemblies,partial trust scenarios
- dynamic assemblies, security
ms.assetid: 0f8bf8fa-b993-478f-87ab-1a1a7976d298
ms.openlocfilehash: 859c564e107fd3a9b219d71dc6ac5ccdf6e9d690
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96259273"
---
# <a name="security-issues-in-reflection-emit"></a>反射发出中的安全问题

.NET Framework 提供了三种发出 Microsoft 中间语言 (MSIL) 的方式，每种方式都有其自身的安全问题：  
  
- [动态程序集](#Dynamic_Assemblies)  
  
- [匿名托管的动态方法](#Anonymously_Hosted_Dynamic_Methods)  
  
- [与现有程序集关联的动态方法](#Dynamic_Methods_Associated_with_Existing_Assemblies)  
  
 无论采用何种方式生成动态代码，执行生成的代码都需要生成代码使用的类型和方法所需的所有权限。  
  
> [!NOTE]
> 反射和发出代码所需的权限已在 .NET Framework 的后续版本中更改。 请参阅本主题后面的[版本信息](#Version_Information)。  
  
<a name="Dynamic_Assemblies"></a>

## <a name="dynamic-assemblies"></a>动态程序集  

 动态程序集是使用 <xref:System.AppDomain.DefineDynamicAssembly%2A?displayProperty=nameWithType> 方法的重载创建的。 此方法的大多数重载在 .NET Framework 4 中已弃用，原因是取消了计算机范围的安全策略。 （请参阅[安全更改](/previous-versions/dotnet/framework/security/security-changes)。）其余的重载可由任意代码执行，而无论其信任级别如何。 这些重载分为两组：一组重载指定在创建动态程序集时要对该程序集应用的特性的列表，另一组重载则不会进行相应的指定。 如果没有通过在创建程序集时应用 <xref:System.Security.SecurityRulesAttribute> 属性来指定程序集的透明度模型，则从发出程序集继承透明度模型。  
  
> [!NOTE]
> 对于在创建动态程序集之后通过使用 <xref:System.Reflection.Emit.AssemblyBuilder.SetCustomAttribute%2A> 方法对该程序集应用的特性，只有在将该程序集保存到磁盘并再次加载到内存中之后，这些特性才会起作用。  
  
 动态程序集中的代码可访问其他程序集中的可见类型和成员。  
  
> [!NOTE]
> 动态程序集不使用 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 和 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志，这些标志允许动态方法访问非公共类型和成员。  
  
 瞬态动态程序集在内存中创建，从不保存到磁盘中，因此它们并不需要文件访问权限。 将动态程序集保存到磁盘需要带有相应标志的 <xref:System.Security.Permissions.FileIOPermission>。  
  
### <a name="generating-dynamic-assemblies-from-partially-trusted-code"></a>从部分受信任的代码生成动态程序集  

 请考虑具有 Internet 权限的程序集可以生成瞬态动态程序集并执行其代码的条件：  
  
- 动态程序集仅使用其他程序集的公共类型和成员。  
  
- 这些类型和成员所需的权限包含在部分受信任的程序集的授予集中。  
  
- 程序集不保存到磁盘。  
  
- 不生成调试符号。 （`Internet` 和 `LocalIntranet` 权限集未包括必要的权限。）  
  
<a name="Anonymously_Hosted_Dynamic_Methods"></a>

## <a name="anonymously-hosted-dynamic-methods"></a>匿名托管的动态方法  

 通过使用未指定关联类型或模块的两个 <xref:System.Reflection.Emit.DynamicMethod> 构造函数（即 <xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%29> 和 <xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%2CSystem.Boolean%29>）来创建匿名托管的动态方法。 这些构造函数将动态方法放到系统提供的完全信任的安全透明的程序集中。 使用这些构造函数或发出动态方法的代码不需要任何权限。  
  
 相反，创建一个匿名托管的动态方法后，即捕获调用堆栈。 构造该方法时，将根据捕获的调用堆栈执行安全请求。  
  
> [!NOTE]
> 从概念上来说，在方法的构造过程中执行请求。 即，可在发出各 MSIL 指令时执行请求。 在当前实现中，当调用 <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A?displayProperty=nameWithType> 方法，或调用实时 (JIT) 编译器（如果在没有调用 <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A> 的情况下调用此方法）时将执行所有请求。  
  
 如果应用程序域允许此操作，匿名托管动态方法可以跳过 JIT 可见性检查，但受到以下限制：匿名托管动态方法访问的非公共类型和成员必须位于其授予集等于发出调用堆栈的授予集或其子集中。 如果应用程序域授予带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>，则启用此跳过 JIT 可见性检查的受限能力。  
  
- 如果你的方法仅使用公共类型和成员，则在构造过程中不需要任何权限。  
  
- 如果指定应跳过 JIT 可见性检查，则在构造方法时执行的请求包括带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>，以及包含正在访问的非公共成员的程序集的授予集。  
  
 由于考虑到非公共成员的授予集，因此被授予 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 的部分受信任代码无法通过执行受信任程序集的非公共成员来提升其特权。  
  
 与任何其他发出的代码一样，执行动态方法需要动态方法使用的方法所需的所有权限。  
  
 托管匿名托管的动态方法的系统程序集使用 <xref:System.Security.SecurityRuleSet.Level1?displayProperty=nameWithType> 透明度模型，这是在 .NET Framework 4 之前的 .NET Framework 中使用的透明度模型。  
  
 有关更多信息，请参见 <xref:System.Reflection.Emit.DynamicMethod> 类。  
  
### <a name="generating-anonymously-hosted-dynamic-methods-from-partially-trusted-code"></a>从部分受信任的代码中生成匿名托管的动态方法  

 请考虑具有 Internet 权限的程序集可以生成匿名托管的动态方法并执行该方法的条件：  
  
- 动态方法仅使用公共类型和成员。 如果动态方法的授予集包含 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType>，则动态方法可以使用任何其授予集等于发出程序集的授予集（或等于发出程序集的授予集的子集）的程序集的非公共类型和成员。  
  
- 动态方法使用的所有类型和成员所需的权限包含在部分受信任的程序集的授予集中。  
  
> [!NOTE]
> 动态方法不支持调试符号。  
  
<a name="Dynamic_Methods_Associated_with_Existing_Assemblies"></a>

## <a name="dynamic-methods-associated-with-existing-assemblies"></a>与现有程序集关联的动态方法  

 若要将动态方法与现有程序集中的某一类型或模块关联，请使用指定关联类型或模块的任一 <xref:System.Reflection.Emit.DynamicMethod> 构造函数。 调用这些构造函数所需的权限各不相同，这是因为将动态方法与现有类型或模块关联会授予该动态方法访问非公共类型和成员的权限：  
  
- 与某一类型关联的动态方法具有对该类型的所有成员（甚至私有成员）以及包含此关联类型的程序集中的所有内部类型和成员的访问权限。  
  
- 与某一模块相关联的态方法具有对该模块中的所有 `internal` 类型和成员（在 Visual Basic 中为 `Friend`，在公共语言运行时元数据中为 `assembly`）的访问权限。  
  
 此外，你可以使用一个构造函数来指定跳过 JIT 编译器的可见性检查的能力。 执行此操作可向动态方法授予对所有程序集中的所有类型和成员的访问权限，而无论访问级别如何。  
  
 构造函数所需的权限取决于你决定向动态方法授予多少访问权限：  
  
- 如果你的方法仅使用公共类型和成员，并且你将该方法与你自己的类型或模块关联，则不需要任何权限。  
  
- 如果指定应跳过 JIT 可见性检查，则构造函数需要带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。  
  
- 如果将动态方法与另一类型（甚至是你自己的程序集中的另一类型）关联，则构造函数需要带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission> 和带有 <xref:System.Security.Permissions.SecurityPermissionFlag.ControlEvidence?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.SecurityPermission>。  
  
- 如果将动态方法与另一程序集中的类型或模块关联，则构造函数可能需要下列两项：带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission> 和包含其他模块的程序集的授予集。 也就是说，调用堆栈必须包括目标模块的授权集中的所有权限以及 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType>。  
  
    > [!NOTE]
    > 为实现向后兼容性，如果对目标授予集和 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 的请求失败，构造函数将需要带有 <xref:System.Security.Permissions.SecurityPermissionFlag.ControlEvidence?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.SecurityPermission>。  
  
 虽然此列表中的项是用发出程序集的授予集来描述的，但是请记住，请求是根据完整的调用堆栈（包括应用程序域边界）来执行的。  
  
 有关更多信息，请参见 <xref:System.Reflection.Emit.DynamicMethod> 类。  
  
### <a name="generating-dynamic-methods-from-partially-trusted-code"></a>从部分受信任的代码生成动态方法  
  
> [!NOTE]
> 从部分受信任的代码生成动态方法的推荐方式是使用[匿名托管的动态方法](#Anonymously_Hosted_Dynamic_Methods)。  
  
 请考虑具有 Internet 权限的程序集可以生成并执行动态方法的条件：  
  
- 动态方法要么与发出它的模块或类型相关联，要么其授予集中包含 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType>，并且该方法与其授予集等于发出程序集的授予集（或者等于发出程序集的授予集的子集）的程序集中的模块关联。  
  
- 动态方法仅使用公共类型和成员。 如果动态方法的授予集包含 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType>，并且该方法与其授予集等于发出程序集的授予集（或者等于发出程序集的授予集的子集）的程序集中的模块关联，那么该方法可使用关联模块中标记为 `internal` 的类型和成员（在 Visual Basic 中为 `Friend`，在公共语言运行时元数据中为 `assembly`）。  
  
- 动态方法使用的所有类型和成员所需的权限包含在部分受信任的程序集的授予集中。  
  
- 动态方法不会跳过 JIT 可见性检查。  
  
> [!NOTE]
> 动态方法不支持调试符号。  
  
<a name="Version_Information"></a>

## <a name="version-information"></a>版本信息  

 从 .NET Framework 4 开始，已取消计算机范围的安全策略，并且安全透明度已成为默认的强制机制。 请参阅[安全更改](/previous-versions/dotnet/framework/security/security-changes)。  
  
 从 .NET Framework 2.0 Service Pack 1 开始，在发出动态程序集和动态方法时不再需要带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。 所有早期版本的 .NET Framework 都需要此标志。  
  
> [!NOTE]
> 默认情况下，带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission> 包含在 `FullTrust` 和 `LocalIntranet` 命名权限集中，而不是在 `Internet` 权限集中。 因此，在 .NET Framework 的早期版本中，仅当库执行 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit> 的 <xref:System.Security.PermissionSet.Assert%2A> 时才能与 Internet 权限一起使用。 这种库需要进行仔细的安全检查，因为编码错误可能会导致安全漏洞。 .NET Framework 2.0 SP1 允许以部分信任形式发出代码而无需发出任何安全请求，因为生成代码本身不是一项特权操作。 也就是说，生成的代码不会具有比发出它的程序集更多的权限。 这使得发出代码的库是安全透明的，且不再需要断言 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit>，这简化了编写安全库任务。  
  
 此外，.NET Framework 2.0 SP1 引入了 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标志，用于从部分受信任的动态方法访问非公共类型和成员。 .NET Framework 的早期版本需要访问非公共类型和成员的动态方法的 <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> 标志；绝不会将该权限授予部分受信任的代码。  
  
 最后，.NET Framework 2.0 SP1 引入了匿名托管的方法。  
  
### <a name="obtaining-information-on-types-and-members"></a>获取有关类型和成员的信息  

 从 .NET Framework 2.0 开始，获取有关非公共类型和成员信息不再需要任何权限。 使用反射可获取发出动态方法所需的信息。 例如，使用 <xref:System.Reflection.MethodInfo> 对象发出方法调用。 .NET Framework 的早期版本需要使用带有 <xref:System.Security.Permissions.ReflectionPermissionFlag.TypeInformation?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。 有关详细信息，请参阅[反射的安全注意事项](security-considerations-for-reflection.md)。  
  
## <a name="see-also"></a>请参阅

- [反射的安全注意事项](security-considerations-for-reflection.md)
- [发出动态方法和程序集](emitting-dynamic-methods-and-assemblies.md)
