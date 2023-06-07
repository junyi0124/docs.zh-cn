---
title: 演练：在部分信任应用场景中发出代码
description: 了解如何在部分信任应用场景中发出代码。 反射发出使用相同的 API，但某些功能在部分受信任代码中需要特殊权限。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- reflection emit, anonymously hosted dynamic methods
- partial trust, reflection
- partial trust, emitting dynamic methods
- reflection emit, partial trust scenarios
- anonymously hosted dynamic methods [.NET Framework]
- emitting dynamic assemblies,partial trust scenarios
- reflection emit, dynamic methods
- dynamic methods
ms.assetid: c45be261-2a9d-4c4e-9bd6-27f0931b7d25
ms.openlocfilehash: 70adb3ce67b45459b18741948092a912f6173731
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865185"
---
# <a name="walkthrough-emitting-code-in-partial-trust-scenarios"></a>演练：在部分信任应用场景中发出代码

反射发出以完全信任或部分信任形式使用相同的 API 集，但某些功能在部分受信任代码中需要特殊权限。 此外，反射发出具有一个功能，即匿名托管动态方法，旨在由安全透明的程序集采用部分信任的形式使用。

> [!NOTE]
> 在 .NET Framework 3.5 之前，发出代码需要带 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType> 标志的 <xref:System.Security.Permissions.ReflectionPermission>。 默认情况下，此权限包含在 `FullTrust` 和 `Intranet` 命名权限集中，而不在 `Internet` 权限集中。 因此，只有当库具有 <xref:System.Security.SecurityCriticalAttribute> 特性并对 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit> 执行了 <xref:System.Security.PermissionSet.Assert%2A> 方法时，才能在部分信任环境下使用该库。 这种库需要进行仔细的安全检查，因为编码错误可能会导致安全漏洞。 .NET Framework 3.5 允许以部分信任形式发出代码而无需发出任何安全请求，因为生成代码本身不是一项特权操作。 也就是说，生成的代码不会具有比发出它的程序集更多的权限。 这使发出代码的库具有安全-透明性且无需断言 <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit>，因此编写安全库无需彻底的安全检查。

本演练阐释了以下任务：

- [为测试部分受信任的代码设置简单沙盒](#Setting_up)。

  > [!IMPORTANT]
  > 这是一种以部门信任形式试验代码的简单方法。 要运行实际来自不可信位置的代码，请参阅[如何：运行沙盒中部分受信任的代码](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)。

- [在部分受信任的应用程序域中运行代码](#Running_code)。

- [使用匿名托管动态方法以部分信任形式发出和执行代码](#Using_methods)。

有关在部分信任方案中发出代码的详细信息，请参阅[反射发出中的安全问题](security-issues-in-reflection-emit.md)。

有关这些过程中所显示代码的完整列表，请参阅本演练末的[示例部分](#Example)。

<a name="Setting_up"></a>

## <a name="setting-up-partially-trusted-locations"></a>设置部分信任的位置

以下两个过程显示如何设置可以测试部分信任的代码的位置。

- 过程一演示如何创建沙盒应用程序域，其中已授予代码 Internet 权限。

- 过程二演示如何将带 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标记的 <xref:System.Security.Permissions.ReflectionPermission> 添加到部分信任的应用程序域，以便访问信任级别相同或更低的程序集中的专用数据。

### <a name="creating-sandboxed-application-domains"></a>创建沙盒应用程序域

要创建使用部分信任形式运行程序集的应用程序域，必须使用 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> 方法重载来指定要授予程序集的权限集，以创建应用程序域。 指定授予集最简单的方法是从安全策略检索命名权限集。

下列过程创建沙盒应用程序域，它运行部分信任的代码，从而测试方案，方案中发出的代码只能访问公共类型的公共成员。 随后的过程演示如何添加 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> 来测试方案，方案中发出的代码可以访问程序集中的非公共类型和成员（授予了程序集相同权限或较低权限）。

#### <a name="to-create-an-application-domain-with-partial-trust"></a>创建部分信任的应用程序域

1. 创建一个权限集，将其授予沙盒应用程序域中的程序集。 在这种情况下，使用的是 Internet 区域的权限集。

    [!code-csharp[HowToEmitCodeInPartialTrust#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#2)]
    [!code-vb[HowToEmitCodeInPartialTrust#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#2)]

2. 创建一个 <xref:System.AppDomainSetup> 对象，使用应用程序路径初始化应用程序域。

    > [!IMPORTANT]
    > 为简单起见，此代码示例使用当前文件夹。 要运行实际来自 Internet 的代码，请为不受信任的代码使用单独的文件夹，如[如何：运行沙盒中部分受信任的代码](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)中所述。

    [!code-csharp[HowToEmitCodeInPartialTrust#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#3)]
    [!code-vb[HowToEmitCodeInPartialTrust#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#3)]

3. 创建应用程序域，为在应用程序域中执行的所有程序集指定应用程序域设置信息和授予集。

    [!code-csharp[HowToEmitCodeInPartialTrust#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#5)]
    [!code-vb[HowToEmitCodeInPartialTrust#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#5)]

    可通过 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> 方法重载的最后一个参数来指定要授予完全信任的一组程序集，而不是应用程序域的授予集。 无需指定应用程序所使用的 .NET Framework 程序集，因为它们位于全局程序集缓存中。 全局程序集缓存中的程序集始终是完全受信任的。 可使用此参数指定全局程序集缓存之外的强名称程序集。

### <a name="adding-restrictedmemberaccess-to-sandboxed-domains"></a>将 RestrictedMemberAccess 添加到沙盒域

主机应用程序可允许匿名托管动态方法访问程序集中的专用数据，该程序集具有与发出代码的程序集相同或更低的信任级别。 要启用此受限能力跳过实时 (JIT) 可见性检查，主机应用程序需使用 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> (RMA) 标记向授予集添加 <xref:System.Security.Permissions.ReflectionPermission> 对象。

例如，主机可能授予 Internet 应用程序 Internet 权限和 RMA，以便 Internet 应用程序能发出访问自身程序集中专用数据的代码。 由于访问仅限于具有相同和较低信任级别的程序集，因此 Internet 应用程序无法访问完全信任的程序集（如 .NET Framework 程序集）的成员。

> [!NOTE]
> 为防止特权提升，在构造匿名托管动态方法时，将包含发出程序集的堆栈信息。 调用方法时检查堆栈信息。 因此，从完全信任的代码调用的匿名托管动态方法仍被限制为发出程序集的信任等级。

#### <a name="to-create-an-application-domain-with-partial-trust-plus-rma"></a>创建部分信任的应用程序域和 RMA

1. 创建一个具有 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> (RMA) 标记的 <xref:System.Security.Permissions.ReflectionPermission> 新对象，并使用 <xref:System.Security.PermissionSet.SetPermission%2A?displayProperty=nameWithType> 方法向授予集添加该权限。

    [!code-csharp[HowToEmitCodeInPartialTrust#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#7)]
    [!code-vb[HowToEmitCodeInPartialTrust#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#7)]

    如果授予集中尚未包含该权限，则 <xref:System.Security.PermissionSet.AddPermission%2A> 方法向其中添加该权限。 如果授予集中已包含该权限，则向现有权限添加指定的标记。

    > [!NOTE]
    > RMA 是匿名托管动态方法的一项功能。 当普通动态方法跳过 JIT 可见性检查时，发出的代码要求完全信任。

2. 创建应用程序域，指定应用程序域设置信息和授予集。

    [!code-csharp[HowToEmitCodeInPartialTrust#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#8)]
    [!code-vb[HowToEmitCodeInPartialTrust#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#8)]

<a name="Running_code"></a>

## <a name="running-code-in-sandboxed-application-domains"></a>在沙盒应用程序域中运行代码

下列过程说明如何使用可在应用程序域中执行的方法来定义类，如何在域中创建类的实例，以及如何执行其方法。

#### <a name="to-define-and-execute-a-method-in-an-application-domain"></a>在应用程序域中定义和执行方法

1. 定义一个从 <xref:System.MarshalByRefObject> 派生的类。 这使你可在其它应用程序域中创建类的实例，并在应用程序域边界之间进行方法调用。 此例中将该类命名为 `Worker`。

    [!code-csharp[HowToEmitCodeInPartialTrust#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#10)]
    [!code-vb[HowToEmitCodeInPartialTrust#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#10)]

2. 定义包含要执行的代码的公共方法。 在此示例中，代码发出一个简单的动态方法，创建一个委托来执行方法，并调用该委托。

    [!code-csharp[HowToEmitCodeInPartialTrust#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#11)]
    [!code-vb[HowToEmitCodeInPartialTrust#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#11)]

3. 在主程序中，获取程序集的显示名称。 在沙盒应用程序域中创建 `Worker` 类的实例时，将用到此名称。

    [!code-csharp[HowToEmitCodeInPartialTrust#14](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#14)]
    [!code-vb[HowToEmitCodeInPartialTrust#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#14)]

4. 在主程序中，按本演练[过程一](#Setting_up)中所述，创建一个沙盒应用程序域。 无需向 `Internet` 权限集添加任何权限，因为 `SimpleEmitDemo` 方法仅使用公共方法。

5. 在主程序中，在沙盒应用程序域中创建 `Worker` 类的实例。

    [!code-csharp[HowToEmitCodeInPartialTrust#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#12)]
    [!code-vb[HowToEmitCodeInPartialTrust#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#12)]

    <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> 方法在目标应用程序域中创建该对象，并返回一个可用于调用该对象的属性和方法的代理。

    > [!NOTE]
    > 如果在 Visual Studio 中使用该代码，则必须更改类名，使其包括命名空间。 默认情况下，命名空间是项目的名称。 例如，如果项目是“PartialTrust”，那么类名必须为“PartialTrust.Worker”。

6. 添加用于调用 `SimpleEmitDemo` 方法的代码。 跨应用程序域边界对调用进行封送，并在沙盒应用程序域中执行该代码。

    [!code-csharp[HowToEmitCodeInPartialTrust#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#13)]
    [!code-vb[HowToEmitCodeInPartialTrust#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#13)]

<a name="Using_methods"></a>

## <a name="using-anonymously-hosted-dynamic-methods"></a>使用匿名托管动态方法

匿名托管动态方法与系统提供的透明程序集相关联。 因此，它们所包含的代码是透明的。 普通动态方法则相反，它必须与现有模块（无论是直接指定，还是从关联的类型推断而出）相关联，并从该模块获取安全级别。

> [!NOTE]
> 将动态方法与提供匿名托管的程序集相关联的唯一途径，是使用下列过程中所述的构造函数。 在匿名托管程序集中，不可显式指定模块。

普通动态方法可以访问与之相关联的模块的内部成员，或访问与之相关联的类型的私有成员。 由于匿名托管动态方法独立于其他代码，因此它们无需访问专用数据。 但是，它们拥有跳过 JIT 可见性检查，访问专用数据的有限能力。 此功能仅限具有与发出代码的程序集相同或较低信任等级的程序集。

为防止特权提升，在构造匿名托管动态方法时，将包含发出程序集的堆栈信息。 调用方法时检查堆栈信息。 从完全信任的代码调用的匿名托管动态方法仍被限制为发出该方法的程序集的信任等级。

#### <a name="to-use-anonymously-hosted-dynamic-methods"></a>使用匿名托管动态方法

- 使用不指定关联模块或类型的构造函数，创建匿名托管动态方法。

  [!code-csharp[HowToEmitCodeInPartialTrust#15](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#15)]
  [!code-vb[HowToEmitCodeInPartialTrust#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#15)]

  如果匿名托管动态方法仅使用公共类型和方法，则它无需受限的成员访问，且无需跳过 JIT 可见性检查。

  发出动态方法无需任何特殊权限，但发出的代码需要它使用的类型和方法所需的权限。 例如，如果发出的代码调用访问某个文件的方法，则它需要 <xref:System.Security.Permissions.FileIOPermission>。 如果信任级别不包含该权限，那么在执行发出的代码时，将引发安全性异常。 此处显示的代码发出一个动态方法，该方法仅使用 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 方法。 因此，可以从部分信任的位置执行该代码。

- 或者，使用 <xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%2CSystem.Boolean%29> 构造函数并为 `restrictedSkipVisibility` 参数指定 `true`，从而创建一个匿名托管动态方法，该方法具有跳过 JIT 可见性检查的受限能力。

  [!code-csharp[HowToEmitCodeInPartialTrust#16](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#16)]
  [!code-vb[HowToEmitCodeInPartialTrust#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#16)]

  限制在于，匿名托管动态方法仅可访问具有与发出程序集相同或比其更低的信任级别的程序集中的专用数据。 例如，如果通过 Internet 信任来执行动态方法，则它可以访问同样以 Internet 信任执行的其他程序集中的专用数据，但无法访问 .NET Framework 程序集中的专用数据。 .NET Framework 程序集安装在全局程序集缓存中，且始终是完全信任的。

  仅当主机应用程序通过 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 标记授予 <xref:System.Security.Permissions.ReflectionPermission> 时，匿名托管动态方法才可使用此受限能力跳过 JIT 可见性检查。 调用方法时发出对此权限的请求。

  > [!NOTE]
  > 构造动态方法时，包括发出程序集的调用堆栈信息。 因此，是针对发出程序集的权限提出请求，而不是针对调用方法的程序集提出请求。 这样可防止使用提升的权限执行发出的代码。

  此演练末的[完整代码示例](#Example)说明了受限成员访问的使用和限制。 它的 `Worker` 类包括可创建具有或不具有跳过可见性检查这一受限能力的匿名托管动态方法，且示例显示在具有不同信任级别的应用程序域中执行此方法的结果。

  > [!NOTE]
  > 跳过可见性检查这一受限能力是匿名托管动态方法的一种功能。 当普通动态方法跳过 JIT 可见性检查时，向它们授予的信任级别必须是完全信任。

<a name="Example"></a>

## <a name="example"></a>示例

### <a name="description"></a>描述

下列代码示例演示如何使用 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> 标记允许匿名托管动态方法跳过 JIT 可见性检查，但仅当目标成员与发出代码的程序集具有相同或较低信任级别时才适用。

此示例定义了可跨应用程序域边界封送的 `Worker` 类。 该类具有两个发出和执行动态方法的 `AccessPrivateMethod` 方法重载。 第一个重载发出调用 `Worker` 类的私有 `PrivateMethod` 方法的动态方法，而且它发出具有或不具有 JIT 可见性检查的动态方法。 第二个重载发出访问 <xref:System.String> 类的 `internal` 属性（Visual Basic 中的 `Friend` 属性）的动态方法。

示例使用 Helper 方法创建限制为 `Internet` 权限的授予集，然后创建应用程序域，使用 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> 方法重载来指定域中执行的全部代码均使用此授予集。 此示例在应用程序域中创建 `Worker` 类的一个实例，并执行 `AccessPrivateMethod` 方法两次。

- 首次执行 `AccessPrivateMethod` 方法时，强制执行 JIT 可见性检查。 动态方法将在被调用时失败，这是由于 JIT 可见性检查阻止其访问私有方法。

- 第二次执行 `AccessPrivateMethod` 方法时，跳过 JIT 可见性检查。 动态方法将在被编译时失败，这是由于 `Internet` 授予集未授予充足的权限，不能跳过可见性检查。

此示例向授予集添加具有 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 的 <xref:System.Security.Permissions.ReflectionPermission>。 然后创建第二个域，指定授予在此域中执行的所有代码新授予集中的权限。 此示例在新应用程序域中创建 `Worker` 类的一个实例，并执行 `AccessPrivateMethod` 方法的两个重载。

- 执行 `AccessPrivateMethod` 方法的第一个重载，跳过 JIT 可见性检查。 动态方法将成功编译和执行，这是由于发出代码的程序集与包含私有方法的程序集相同。 因此，其信任级别相同。 如果包含 `Worker` 类的应用程序拥有若干程序集，则对于任何一个程序集，相同的进程都将成功，因为它们具有相同的信任级别。

- 执行 `AccessPrivateMethod` 方法的第二个重载，再次跳过 JIT 可见性检查。 这一次，动态方法在编译时失败，因为它尝试访问 <xref:System.String> 类的 `internal` `FirstChar` 属性。 包含 <xref:System.String> 类的程序集是完全信任的程序集。 因此，它比发出代码的程序集拥有更高的信任级别。

此比较显示了 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> 如何使部分受信任的代码为其它部分受信任的代码跳过可见性检查，而不影响受信任代码的安全性。

### <a name="code"></a>代码

[!code-csharp[HowToEmitCodeInPartialTrust#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#1)]
[!code-vb[HowToEmitCodeInPartialTrust#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#1)]

## <a name="compiling-the-code"></a>编译代码

- 如果在 Visual Studio 中生成此代码示例，则必须更改类名，以便在将命名空间传递给 <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> 方法时包括该命名空间。 默认情况下，命名空间是项目的名称。 例如，如果项目是“PartialTrust”，那么类名必须为“PartialTrust.Worker”。

## <a name="see-also"></a>请参阅

- [反射发出中的安全问题](security-issues-in-reflection-emit.md)
- [如何：运行沙盒中部分受信任的代码](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)
