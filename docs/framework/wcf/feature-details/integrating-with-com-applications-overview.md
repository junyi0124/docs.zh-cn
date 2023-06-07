---
title: COM 应用程序集成概述
ms.date: 03/30/2017
helpviewer_keywords:
- COM [WCF], integration overview
ms.assetid: 02c5697f-6e2e-47d6-b715-f3a28aebfbd5
ms.openlocfilehash: c283e7cbc4cb4b8bc37dd1313480410df93a93bf
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596820"
---
# <a name="integrating-with-com-applications-overview"></a>COM 应用程序集成概述

Windows Communication Foundation （WCF）为托管代码开发人员提供了一个用于创建连接的应用程序的丰富环境。 但是，如果你对基于 COM 的非托管代码进行了大量投资并且不想迁移，则仍可以使用 WCF 服务名字对象将 WCF Web 服务直接集成到现有代码中。 服务标记可以在众多基于 COM 的开发环境（如 Office VBA、Visual Basic 6.0 或 Visual C++ 6.0）中使用。

> [!NOTE]
> 服务标记使用 WCF 信道来进行所有通信。 用于该通道的安全和标识机制不同于标准 COM 和 DCOM 代理中使用的机制。 此外，由于服务标记使用 WCF 信道，因此所有调用的默认超时期限为一分钟。

服务标记与函数结合使用， `GetObject` 为非托管开发人员提供强类型的、特定于 COM 的方法来调用 WCF Web 服务。 这需要 WCF Web 服务协定的本地、COM 可见的定义以及要使用的绑定。 与其他 WCF 客户端一样，服务标记必须构造一个到服务的类型化通道，但对于第一个方法调用中的 COM 程序员，此通道构造是透明的。

与其他 WCF 客户端一样，在使用名字对象时，应用程序会指定地址、绑定和协定，以与服务进行通信。 可以采用以下方式之一来指定协定：

- 类型化协定 – 该协定在客户端计算机上注册为 COM 可见类型。

- WSDL 协定 – 该协定以 WSDL 文档的形式提供。

- MEX 协定 – 在运行时从元数据交换 (MEX) 终结点检索协定。

## <a name="parameters-supported-by-the-service-moniker"></a>服务标记支持的参数

下表显示了服务标记支持的参数。

|参数|描述|
|---------------|-----------------|
|`address`|服务的 URL 位置。|
|`binding`|应用程序配置中的绑定节名。|
|`bindingConfiguration`|命名绑定节中的命名绑定实例。|
|`contract`|表示服务协定或协定名称（MEX 中）的接口标识符 (IID)。|
|`wsdl`|提供协定定义的替代形式的 WSDL 文档。|
|`spnIdentity`|用于与服务通信的服务器主体名称 (SPN) 标识。|
|`upnIdentity`|用于与服务通信的用户主体名称 (UPN) 标识。|
|`dnsIdentity`|用于与服务通信的 DNS 标识。|
|`mexAddress`|服务的元数据交换 (MEX) 终结点的 URL 位置。|
|`mexBinding`|应用程序配置中用于与 MEX 终结点连接的绑定节名。|
|`mexBindingConfiguration`|命名绑定节中用于与 MEX 终结点连接的命名绑定实例。|
|`bindingNamespace`|检索的 MEX 中绑定节名的命名空间。|
|`contractNamespace`|检索的 MEX 中协定的命名空间。|
|`mexSpnIdentity`|用于与 MEX 终结点通信的服务器主体名称 (SPN) 标识。|
|`mexUpnIdentity`|用于与 MEX 终结点通信的用户主体名称 (UPN) 标识。|
|`mexDnsIdentity`|用于与 MEX 终结点通信的 DNS 标识。|
|`serializer`|指定“xml”或“datacontract”序列化程序的用法。|

> [!NOTE]
> 即使与完全基于 COM 的客户端一起使用，服务标记也要求在客户端计算机上安装 WCF 和支持的 .NET Framework 2.0。 使用服务标记的客户端应用程序加载 .NET Framework 运行时的适当版本也很关键。 在 Office 应用程序内使用标记时，可能需要一个配置文件以确保加载正确的 Framework 版本。 例如，使用 Excel 时，以下文本应放在与 Excel.exe 文件位于同一目录中的名为 Excel.exe.config 的文件中：
>
> `<?xml version="1.0" encoding="utf-8"?>`
>
> `<configuration xmlns=` `http://schemas.microsoft.com/.NetConfiguration/v2.0` `>`
>
> `<startup>`
>
> `<requiredRuntime version="v2.0.50727" />`
>
> `</startup>`
>
> `</configuration>`

## <a name="see-also"></a>另请参阅

- [如何：注册和配置服务名字对象](how-to-register-and-configure-a-service-moniker.md)
