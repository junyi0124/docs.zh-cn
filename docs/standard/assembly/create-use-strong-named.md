---
title: 创建和使用具有强名称的程序集
description: 本文演示对 .NET 中具有强名称的程序集进行签名以及稍后使用该名称对其进行引用的过程。
ms.date: 08/19/2019
helpviewer_keywords:
- strong-name bypass feature
- strong-named assemblies, about strong-named assemblies
- strong-named assemblies
- signing assemblies
- assemblies [.NET], signing
- strong-named assemblies, scenarios
- assemblies [.NET], strong-named
- strong-named assemblies, loading into trusted application domains
- assembly binding, strong-named
ms.assetid: ffbf6d9e-4a88-4a8a-9645-4ce0ee1ee5f9
ms.openlocfilehash: 1d87edde97e77011b678662f61500c7acd8293b0
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162708"
---
# <a name="create-and-use-strong-named-assemblies"></a>创建和使用具有强名称的程序集

强名称是由程序集的标识加上公钥和数字签名组成的。其中，程序集的标识包括简单文本名称、版本号和区域性信息（如果提供的话）。 它使用相应私钥从程序集文件生成。 （程序集文件包含程序集清单，该清单包含组成程序集的所有文件的名称和哈希。）

> [!WARNING]
> 不要依赖于通过强名称实现安全性。 它们仅提供唯一的标识。

具有强名称的程序集只能使用其他具有强名称的程序集的类型。 否则，将会危害强名称程序集的完整性。

> [!NOTE]
> 虽然 .NET Core 支持强名称程序集，而且 .NET Core 库中的所有程序集均已签名，但大多数第三方程序集不需要强名称。 有关详细信息，请参阅 GitHub 上的[强名称签名](https://github.com/dotnet/runtime/blob/master/docs/project/strong-name-signing.md)。

## <a name="strong-name-scenario"></a>强名称方案

下面的方案概述了对具有强名称的程序集进行签名以及稍后使用该名称对其进行引用的过程。

1. 使用以下方法之一创建具有强名称的程序集 A：

    - 使用支持创建强名称的开发环境，如 Visual Studio。

    - 使用[强名称工具 (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) 创建加密密钥对，并使用命令行编译器或[程序集链接器 (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) 将密钥对分配到程序集。 Windows SDK 同时提供 Sn.exe 和 Al.exe。

2. 开发环境或工具对包含具有开发人员私钥的程序集清单的文件的哈希进行签名。 此数字签名存储在包含程序集 A 的清单的可移植可执行 (PE) 文件中。

3. 程序集 B 是程序集 A 的一个使用者。程序集 B 清单的引用部分包含表示程序集 A 公钥的标记。 标记是完整公钥的一部分，并且使用它而不是密钥本身以节省空间。

4. 当程序集放置在全局程序集缓存中时，公共语言运行时验证强名称签名。 在运行时按强名称绑定时，公共语言运行时会将存储在程序集 B 的清单中的密钥与为程序集 A 生成强名称的密钥进行比较。如果 .NET 安全检查通过且绑定成功，程序集 B 就可保证程序集 A 的位未被篡改，且这些位确实来自程序集 A 的开发人员。

> [!NOTE]
> 此方案不解决信任问题。 除强名称外，程序集可携带完整的 Microsoft Authenticode 签名。 Authenticode 签名包括建立信任的证书。 请务必注意强名称不要求代码以这种方式进行签名。 强名称仅提供唯一的标识。

## <a name="bypass-signature-verification-of-trusted-assemblies"></a>跳过受信任程序集的签名验证

从 .NET Framework 3.5 Service Pack 1 开始，当程序集加载到完全信任的应用程序域（如 `MyComputer` 区域的默认应用程序域）时，不会验证强名称签名。 这被称之为强名称跳过功能。 在完全信任的环境中，对于已签名的完全信任的程序集，对 <xref:System.Security.Permissions.StrongNameIdentityPermission> 的需求总是成功，而不考虑其签名。 这种情况下，强名称跳过功能可避免完全信任程序集不必要的强名称签名验证开销，允许更快地加载程序集。

跳过功能适用于使用强名称进行签名及具有以下特征的任何程序集：

- 完全受信任，无需 <xref:System.Security.Policy.StrongName> 证据（如具有 `MyComputer` 区域证据）。

- 加载到完全受信任的 <xref:System.AppDomain>。

- 加载自该 <xref:System.AppDomain> 的 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性下的某个位置。

- 签名没有延迟。

可为单个应用程序或计算机禁用此功能。 请参阅[如何：禁用强名称跳过功能](disable-strong-name-bypass-feature.md)。

## <a name="related-topics"></a>相关主题

|Title|描述|
|-----------|-----------------|
|[如何：创建公钥/私钥对](create-public-private-key-pair.md)|描述如何创建加密密钥对以对程序集进行签名。|
|[如何：使用强名称为程序集签名](sign-strong-name.md)|介绍如何创建具有强名称的程序集。|
|[改进的强命名](enhanced-strong-naming.md)|描述 .NET Framework 4.5 中强名称的改进。|
|[如何：引用具有强名称的程序集](reference-strong-named.md)|介绍如何在编译时或运行时引用具有强名称的程序集中的类型或资源。|
|[如何：禁用强名称跳过功能](disable-strong-name-bypass-feature.md)|描述如何禁用跳过强名称签名验证的功能。 可为所有或特定应用程序禁用此功能。|
|[创建程序集](create.md)|提供单个文件和多文件程序集的概述。|
|[如何在 Visual Studio 中延迟对程序集的签名](/visualstudio/ide/managing-assembly-and-manifest-signing#how-to-sign-an-assembly-in-visual-studio)|说明如何在创建程序集后对具有强名称的程序集进行签名。|
|[Sn.exe（强名称工具）](../../framework/tools/sn-exe-strong-name-tool.md)|介绍 .NET Framework 中包含的可帮助创建具有强名称的程序集的工具。 此工具提供有关密钥管理、签名生成和签名验证的选项。|
|[Al.exe（程序集链接器）](../../framework/tools/al-exe-assembly-linker.md)|介绍 .NET Framework 中包含的一种工具，该工具可生成具有模块或资源文件的程序集清单的文件。|
