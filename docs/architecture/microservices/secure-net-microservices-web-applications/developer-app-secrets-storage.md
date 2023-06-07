---
title: 在开发过程中安全地存储应用程序机密
description: .Net 微服务和 Web 应用程序中的安全性 - 不要在源代码管理中存储密码、连接字符串或 API 密钥等应用程序机密，了解在 ASP.NET Core 中可以使用的选项，特别是必须了解如何处理“用户机密”。
author: mjrousos
ms.date: 01/30/2020
ms.openlocfilehash: 1ef2246746b9165f1564fa7be64ff7eb28eb1d32
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "77501797"
---
# <a name="store-application-secrets-safely-during-development"></a>在开发过程中安全地存储应用程序机密

若要连接受保护的资源和其他服务，ASP.NET Core 应用程序通常需要使用包含敏感信息的连接字符串、密码或其他凭据。 这些敏感的信息片段被称为“机密”  。 最好不要在源代码中包含机密，并确保不要在源代码管理中存储机密。 相反，应使用 ASP.NET Core 配置模型从更安全的位置读取机密。

必须将用于访问开发和暂存资源的机密与用于访问生产资源的机密分开，因为不同的个人需要访问不同的机密集。 若要存储在开发过程中使用的机密，常用的方法是在环境变量中或通过使用 ASP.NET Core 机密管理器工具存储机密。 为了在生产环境中实现更安全的存储，微服务可以将机密存储在 Azure Key Vault 中。

## <a name="store-secrets-in-environment-variables"></a>在环境变量中存储机密

在源代码外部保存机密的一种方法是，开发者将基于字符串的机密设置为开发计算机上的[环境变量](/aspnet/core/security/app-secrets#environment-variables)。 当使用环境变量来存储具有层次结构名称（如嵌套在配置节中的名称）的机密时，必须为这些变量命名，以包含其各节的完整层次结构，并用冒号 (:) 分隔。

例如，将环境变量 `Logging:LogLevel:Default` 设置为 `Debug` 值将等效于以下 JSON 文件中的配置值：

```json
{
    "Logging": {
        "LogLevel": {
            "Default": "Debug"
        }
    }
}
```

若要从环境变量访问这些值，应用程序只需在构造 IConfigurationRoot 对象时调用其 ConfigurationBuilder 上的 AddEnvironmentVariables。

请注意，环境变量通常以纯文本形式存储，因此，如果计算机或带有环境变量的进程受到破坏，环境变量值将可见。

## <a name="store-secrets-with-the-aspnet-core-secret-manager"></a>使用 ASP.NET Core 机密管理器存储机密

ASP.NET Core [机密管理器](/aspnet/core/security/app-secrets#secret-manager)工具提供了开发过程中在源代码外部保存机密的另一种方法  。 若要使用机密管理器工具，请在项目文件中安装包 Microsoft.Extensions.Configuration.SecretManager  。 如果该依赖项存在并且已还原，则可以使用 `dotnet user-secrets` 命令来通过命令行设置机密的值。 这些机密将存储在用户配置文件目录中的 JSON 文件中（详细信息随操作系统而异），与源代码无关。

机密管理器工具设置的机密是由使用机密的项目的 `UserSecretsId` 属性组织的。 因此，必须确保在项目文件中设置 UserSecretsId 属性，如下面的代码片段所示。 默认值是 Visual Studio 分配的 GUID，但实际字符串并不重要，只要它在计算机中是唯一的。

```xml
<PropertyGroup>
    <UserSecretsId>UniqueIdentifyingString</UserSecretsId>
</PropertyGroup>
```

通过调用 ConfigurationBuilder 实例上的 `AddUserSecrets<T>` 将应用程序的机密包含在其配置中，就可以在应用程序中使用通过机密管理器存储的机密。 泛型参数 T 应是 UserSecretId 应用到的程序集中的类型。 通常使用 `AddUserSecrets<Startup>` 没问题。

在 Program.cs 中使用 `CreateDefaultBuilder` 方法时，`AddUserSecrets<Startup>()` 包含在开发环境的默认选项中。

>[!div class="step-by-step"]
>[上一页](authorization-net-microservices-web-applications.md)
>[下一页](azure-key-vault-protects-secrets.md)
