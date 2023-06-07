---
title: 代码访问安全策略兼容性和迁移
description: 阅读摘要，查看有关代码访问安全策略兼容性和迁移的链接（.NET Framework 4）。
ms.date: 03/30/2017
helpviewer_keywords:
- policy migration, compatibility
- CLR policy migration
ms.assetid: 19cb4d39-e38a-4262-b507-458915303115
ms.openlocfilehash: 389976556175c0b6b300e75d01327d91f94f0db9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733381"
---
# <a name="code-access-security-policy-compatibility-and-migration"></a>代码访问安全策略兼容性和迁移

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

代码访问安全性 (CA) 的策略部分在 .NET Framework 4 中已过时。 因此，如果通过) 的其他类型和成员 [显式](#explicit_use) 或 [隐式](#implicit_use) 地 (调用过时的策略类型和成员，则可能会遇到编译警告和运行时异常。

可以通过以下任一方法来避免这些警告和错误：

- [迁移](#migration) 到 .NET Framework 4 替换已过时的调用。

   \- 或 -

- 使用[ \<NetFx40_LegacySecurityPolicy> 配置元素](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md)选择使用旧的 CAS 策略行为。

本主题包含以下各节：

- [显式使用](#explicit_use)

- [隐式使用](#implicit_use)

- [错误和警告](#errors_and_warnings)

- [迁移：替换已过时的调用](#migration)

- [兼容性：使用 CAS 策略旧版选项](#compatibility)

<a name="explicit_use"></a>

## <a name="explicit-use"></a>显式使用

直接操作安全策略或需要沙盒的 CAS 策略的成员已过时，并且默认情况下将产生错误。

示例包括：

- <xref:System.AppDomain.SetAppDomainPolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.HostSecurityManager.DomainPolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.Policy.PolicyLevel.CreateAppDomainLevel%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.LoadPolicyLevelFromString%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.LoadPolicyLevelFromFile%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.ResolvePolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.ResolveSystemPolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.ResolvePolicyGroups%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.PolicyHierarchy%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.SavePolicy%2A?displayProperty=nameWithType>

<a name="implicit_use"></a>

## <a name="implicit-use"></a>隐式使用

由于若干程序集加载重载隐式使用 CAS 策略，因此它们产生错误。 这些重载采用用于解决 CAS 策略的 <xref:System.Security.Policy.Evidence> 参数，并为程序集提供权限授予集。

下面是一些示例。 过时的重载是使用 <xref:System.Security.Policy.Evidence> 作为参数的重载：

- <xref:System.Activator.CreateInstanceFrom%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.CreateInstanceFrom%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.CreateInstanceAndUnwrap%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.DefineDynamicAssembly%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.ExecuteAssemblyByName%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>

<a name="errors_and_warnings"></a>

## <a name="errors-and-warnings"></a>错误和警告

当使用过时的类型和成员时，会产生下列错误消息。 请注意，<xref:System.Security.Policy.Evidence?displayProperty=nameWithType> 类型本身并未过时。

编译时警告：

`warning CS0618: '<API Name>' is obsolete: 'This method is obsolete and will be removed in a future release of the .NET Framework. Please use <suggested alternate API>. See <link> for more information.'`

运行时异常：

<xref:System.NotSupportedException>: `This method uses CAS policy, which has been obsoleted by the .NET Framework. In order to enable CAS policy for compatibility reasons, please use the <NetFx40_LegacySecurityPolicy> configuration switch. Please see <link> for more information.`

<a name="migration"></a>

## <a name="migration-replacement-for-obsolete-calls"></a>迁移：替换已过时的调用

### <a name="determining-an-assemblys-trust-level"></a>确定程序集的信任级别

CAS 策略通常用于确定程序集或应用程序域的权限授予集或信任级别。 .NET Framework 4 公开了以下有用的属性，这些属性不需要解析安全策略：

- <xref:System.Reflection.Assembly.PermissionSet%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.IsFullyTrusted%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.PermissionSet%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.IsFullyTrusted%2A?displayProperty=nameWithType>

### <a name="application-domain-sandboxing"></a>应用程序域沙盒

<xref:System.AppDomain.SetAppDomainPolicy%2A?displayProperty=nameWithType> 方法通常用于对应用程序域中的程序集进行沙盒处理。 .NET Framework 4 公开无需用于 <xref:System.Security.Policy.PolicyLevel> 此目的的成员。 有关详细信息，请参阅 [如何：在沙盒中运行部分受信任的代码](how-to-run-partially-trusted-code-in-a-sandbox.md)。

### <a name="determining-a-safe-or-reasonable-permission-set-for-partially-trusted-code"></a>确定部分受信任的代码的“安全”或“合理”权限集

主机通常需要确定适用于沙盒处理托管代码的权限。 在 .NET Framework 4 之前，CAS 策略提供了一种方法来执行此操作 <xref:System.Security.SecurityManager.ResolvePolicy%2A?displayProperty=nameWithType> 。 作为替代，.NET Framework 4 提供 <xref:System.Security.SecurityManager.GetStandardSandbox%2A?displayProperty=nameWithType> 方法，该方法为所提供的证据返回安全的标准权限集。

### <a name="non-sandboxing-scenarios-overloads-for-assembly-loads"></a>非沙盒处理方案：程序集加载的重载

使用程序集加载重载的原因可能是为了使用不可通过其他方式使用的参数，而不是对程序集进行沙盒处理。 从 .NET Framework 4 开始，不需要对象作为参数的程序集加载重载， <xref:System.Security.Policy.Evidence?displayProperty=nameWithType> 例如， <xref:System.AppDomain.ExecuteAssembly%28System.String%2CSystem.String%5B%5D%2CSystem.Byte%5B%5D%2CSystem.Configuration.Assemblies.AssemblyHashAlgorithm%29?displayProperty=nameWithType> 启用此方案。

如果要对程序集进行沙盒处理，请使用 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> 重载。

<a name="compatibility"></a>

## <a name="compatibility-using-the-cas-policy-legacy-option"></a>兼容性：使用 CAS 策略旧版选项

[ \<NetFx40_LegacySecurityPolicy> 配置元素](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md)允许你指定进程或库使用旧版 CAS 策略。 启用此元素时，策略和证据重载将与其在 Framework 以前版本中进行的操作相同。

> [!NOTE]
> CAS 策略行为是在运行时版本的基础上指定的，因此修改一个运行时版本的 CAS 策略不会影响另一个版本的 CAS 策略。

```xml
<configuration>
   <runtime>
      <NetFx40_LegacySecurityPolicy enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [如何：运行沙盒中部分受信任的代码](how-to-run-partially-trusted-code-in-a-sandbox.md)
- [安全编码准则](../../standard/security/secure-coding-guidelines.md)
