---
title: 强命名和 .NET 库
description: 强命名 .NET 库的最佳实践建议。
ms.date: 10/16/2018
ms.openlocfilehash: 6f9533d768331964a8e640243536b12ddde158e5
ms.sourcegitcommit: 7588b1f16b7608bc6833c05f91ae670c22ef56f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2020
ms.locfileid: "93189207"
---
# <a name="strong-naming"></a>强命名

强命名指使用密钥对程序集进行签名，从而生成[强名称程序集](../assembly/strong-named.md)。 程序集具有强名称时，将基于名称和程序集版本号创建唯一标识，并有助于防止发生程序集冲突。

强命名的缺点是，一旦程序集具有强名称，Windows 上的 .NET Framework 就会严格加载程序集。 强名称程序集引用必须与已加载的程序集版本完全匹配，强制开发人员在使用程序集时[配置绑定重定向](../../framework/configure-apps/redirect-assembly-versions.md)：

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
         <dependentAssembly>
            <assemblyIdentity name="myAssembly" publicKeyToken="32ab4ba45e0a69a1" culture="neutral" />
            <bindingRedirect oldVersion="1.0.0.0" newVersion="2.0.0.0"/>
         </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

当 .NET 开发人员抱怨强命名时，通常抱怨的是严格的程序集加载。 幸运的是，此问题独立于 .NET Framework。 .NET 5+、.NET Core、Xamarin、UWP 和大多数其他 .NET 实现没有严格的程序集加载（强命名的主要缺点）。

强命名的一个重要方面是它已迅速传播开了：某个强名称程序集只能引用其他强名称程序集。 如果你的库未强命名，则会阻止生成需要强命名的应用程序或库的开发人员使用该库。

强命名的优点包括：

1. 程序集可以被引用并由其他强名称程序集使用。
2. 程序集可以存储在全局程序集缓存 (GAC) 中。
3. 程序集可以与其他版本的程序集并行加载。 具有插件体系结构的应用程序通常需要并行程序集加载。

## <a name="create-strong-named-net-libraries"></a>创建强名称 .NET 库

应强命名开放源代码 .NET 库。 强命名程序集可确保大多数人可以使用它，并且严格的程序集加载仅影响 .NET Framework。

> [!NOTE]
> 本指南特定于公开分布的 .NET 库，如 NuGet.org 上发布的 .NET 库。强命名不是大多数 .NET 应用程序所必需的，默认情况下不得执行强命名。

✔️ 请考虑强命名库的程序集。

✔️ 请考虑将强命名密钥添加到源代码管理系统。

> 公开提供的密钥可让开发人员修改库源代码并使用相同的密钥进行重新编译。
>
> 如果强命名密钥过去用于在[部分信任的方案](../../framework/misc/using-libraries-from-partially-trusted-code.md)中授予特殊权限，则不得公开该强命名密钥。 或者，可能会破坏现有环境。

> [!IMPORTANT]
> 需要了解代码发布者的身份时，建议使用[验证码](/windows-hardware/drivers/install/authenticode)和 [NuGet 包签名](/nuget/create-packages/sign-a-package)。 代码访问安全性 (CAS) 不得用作安全缓解。

✔️ 请考虑仅在主版本发生更改时递增程序集版本，以帮助用户减少绑定重定向和更新频率。

> 详细了解[版本控制和程序集版本](./versioning.md#assembly-version)。

❌ 请勿添加、删除或更改强命名密钥。

> 修改程序集的强命名密钥会更改程序集的标识并破坏使用该标识的已编译代码。 有关详细信息，请参阅[二进制重大更改](./breaking-changes.md#binary-breaking-change)。

❌ 请勿发布库的强名称和非强名称版本。 例如，`Contoso.Api` 和 `Contoso.Api.StrongNamed`。

> 发布两个包会为开发人员生态系统创建分支。 此外，如果应用程序依赖于这两个包，则开发人员可能会遇到类型名称冲突。 就 .NET 而言，它们是不同程序集中的不同类型。

>[!div class="step-by-step"]
>[上一页](cross-platform-targeting.md)
>[下一页](nuget.md)
