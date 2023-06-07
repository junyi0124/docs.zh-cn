---
title: 理解使用用于 .NET 的 Azure 库进行身份验证
description: 介绍使用用于 .NET 的 Azure SDK 进行身份验证的不同方法。
ms.date: 06/19/2020
ms.custom: devx-track-dotnet
ms.openlocfilehash: 0a508606b40905d5b09b8515910873359efbbf2e
ms.sourcegitcommit: 3d6d6595a03915f617349781f455f838a44b0f44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2020
ms.locfileid: "97700836"
---
# <a name="authenticate-with-the-azure-sdk-for-net"></a>使用用于 .NET 的 Azure SDK 进行身份验证

## <a name="recommended-azureidentity"></a>建议：Azure.Identity

用于 .NET 的 Azure SDK 中最新的包使用常用的身份验证包进行身份验证，即 `Azure.Identity`。 建议使用 `Azure.Identity`，而不是本文档稍后将介绍的其他身份验证机制。 支持 `Azure.Identity` 提供的凭据的包在 `Azure.Core` 的基础之上构建，并且具有开头为 Azure 的包标识符。 有关使用 `Azure.Core` 的包的清单，请参阅[包列表](packages.md)。

若要查看有关在项目中使用 `Azure.Identity` 的完整说明，请参阅[用于 .NET 的 Azure 标识客户端](/dotnet/api/overview/azure/identity-readme)的文档。

> [!TIP]
> 若要了解有关使用 Azure 标识管理和访问 Azure 资源的示例，请参阅 [Azure 标识、资源管理和存储示例](/samples/dotnet/samples/azure-identity-resource-management-storage/)。

若要使用不支持 Azure.Identity 的库进行身份验证，请参阅本主题的其余部分。

## <a name="access-azure-resources"></a>访问 Azure 资源

若要与 Azure 资源进行交互（例如从 Key Vault 检索机密，或在 Azure 存储中存储 Blob），许多 Azure 服务库需要使用连接字符串或密钥进行身份验证。 例如，SQL 数据库使用[标准 SQL 连接字符串](/azure/azure-sql/database/connect-query-dotnet-core)。 服务连接字符串用于 [CosmosDB](/azure/cosmos-db/)、[Azure Cache for Redis](/azure/azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache) 和[服务总线](/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues)等其他 Azure 服务。 可以使用 Azure 门户、CLI 或 PowerShell 获取这些字符串。 还可以使用用于 .NET 的 Azure 管理库来查询资源，以便在代码中生成连接字符串。

使用连接字符串的方法因产品而异。 [请参阅 Azure 产品的文档](/azure/?product=featured)。

## <a name="manage-azure-resources"></a>管理 Azure 资源

[!include[Create service principal](includes/create-sp.md)]

创建服务主体后，可以使用两个选项对服务主体进行身份验证，以创建和管理资源。

若要使用这两个选项，需将以下 NuGet 包添加到项目。

```powershell
Install-Package Microsoft.Azure.Management.Fluent
Install-Package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="authenticate-with-token-credentials"></a>使用令牌凭据进行身份验证

第一种方法是在代码中生成令牌凭据对象。 应将凭据安全存储在配置文件、注册表或 Azure KeyVault 中。

```csharp
var credentials = SdkContext.AzureCredentialsFactory
    .FromServicePrincipal(clientId,
        clientSecret,
        tenantId,
        AzureEnvironment.AzureGlobalCloud);
```

使用在创建服务主体时获得的 JSON 输出中的 *clientId*、*clientSecret* 和 *tenantId* 值。

然后，创建入口点 `Azure` 对象以开始使用 API：

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```

建议向 `Azure` 对象显式提供 JSON 输出中的 subscriptionId：

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithSubscription(subscriptionId);
```

### <a name="file-based-authentication"></a><a name="mgmt-file"></a>基于文件的身份验证

使用基于文件的身份验证可将服务主体凭据放入纯文本文件，并在文件系统中对其进行保护。

[!include[File-based authentication](includes/file-based-auth.md)]

读取该文件的内容，并创建入口点 `Azure` 对象以开始使用 API：

```csharp
// pull in the location of the authentication properties file from the environment
var credentials = SdkContext.AzureCredentialsFactory
    .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```
