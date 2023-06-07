---
title: 错误处理 - GRPC，适用于 WCF 开发人员
description: 与 gRPC 中的错误处理相关的主题。 包括最常用的状态代码的表。
ms.date: 09/02/2019
ms.openlocfilehash: 64a2355a8bd608c074f9bc420312b23aba0c1fb2
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988942"
---
# <a name="error-handling"></a>错误处理。

Windows 通信基础 （WCF） 使用<xref:System.ServiceModel.FaultException%601>和[故障合同](xref:System.ServiceModel.FaultContractAttribute)提供详细的错误信息，包括支持 SOAP 故障标准。

遗憾的是，gRPC 的当前版本缺乏 WCF 的成熟度，并且仅根据简单的状态代码和元数据进行有限的内置错误处理。 下表是最常用的状态代码的快速指南：

| 状态代码 | 问题 |
| ----------- | ------- |
| `GRPC_STATUS_UNIMPLEMENTED` | 方法尚未编写。 |
| `GRPC_STATUS_UNAVAILABLE` | 整个服务有问题。 |
| `GRPC_STATUS_UNKNOWN` | 无效响应。 |
| `GRPC_STATUS_INTERNAL` | 编码/解码问题。 |
| `GRPC_STATUS_UNAUTHENTICATED` | 身份验证失败。 |
| `GRPC_STATUS_PERMISSION_DENIED` | 授权失败。 |
| `GRPC_STATUS_CANCELLED` | 呼叫已取消，通常由呼叫者取消。 |

## <a name="raise-errors-in-aspnet-core-grpc"></a>引发ASP.NET核心 gRPC 中的错误

ASP.NET Core gRPC 服务可以通过引发 一个`RpcException`发送 错误响应来发送错误响应，客户端可以捕获该响应，就像它处于同一进程中一样。 `RpcException`必须包括状态代码和说明，并且可以选择包括元数据和较长的异常消息。 元数据可用于发送支持数据，类似于对象如何`FaultContract`为 WCF 错误携带其他数据。

```csharp
public async Task<GetPortfolioResponse> GetPortfolio(GetPortfolioRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;
    if (!ValidateUser(user))
    {
        var metadata = new Metadata
        {
            { "User", user.Identity.Name }
        };
        throw new RpcException(new Status(StatusCode.PermissionDenied, "Permission denied"), metadata);
    }
}
```

## <a name="catch-errors-in-grpc-clients"></a>捕获 gRPC 客户端中的错误

就像 WCF 客户端可以<xref:System.ServiceModel.FaultException%601>捕获错误一样，gRPC 客户端`RpcException`可以捕获处理错误的。 因为`RpcException`不是泛型类型，因此无法捕获不同块中的不同错误类型。 但是，您可以使用 C# 的*异常筛选器*功能为不同的状态代码`catch`声明单独的块，如以下示例所示：

```csharp
try
{
    var portfolio = await client.GetPortfolioAsync(new GetPortfolioRequest { Id = id });
}
catch (RpcException ex) when (ex.StatusCode == StatusCode.PermissionDenied)
{
    var userEntry = ex.Trailers.FirstOrDefault(e => e.Key == "User");
    Console.WriteLine($"User '{userEntry.Value}' does not have permission to view this portfolio.");
}
catch (RpcException)
{
    // Handle any other error type ...
}
```

> [!IMPORTANT]
> 为错误提供其他元数据时，请确保在 API 文档中或`.proto`文件中的注释中记录相关键和值。

## <a name="grpc-richer-error-model"></a>gRPC 更丰富的错误模型

谷歌已经开发出一个更[丰富的错误模型](https://cloud.google.com/apis/design/errors#error_model)，更像WCF的[故障合同](xref:System.ServiceModel.FaultContractAttribute)，但这种模式在C#中还不受支持。 目前，它仅适用于 Go、Java、Python 和C++。

>[!div class="step-by-step"]
>[上一页](metadata.md)
>[下一页](ws-protocols.md)
