---
title: 教程： Windows Communication Foundation 应用程序入门
description: 以下教程介绍了如何创建 WCF 应用程序。
ms.date: 01/25/2019
helpviewer_keywords:
- WCF [WCF], get started
- Windows Communication Foundation [WCF], get started
- get started [WCF]
ms.assetid: df939177-73cb-4440-bd95-092a421516a1
ms.openlocfilehash: 620a83260f01e27a19b19227165695a621c88e5e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238231"
---
# <a name="tutorial-get-started-with-windows-communication-foundation-applications"></a>教程： Windows Communication Foundation 应用程序入门

以下一系列教程介绍了 Windows Communication Foundation (WCF) 编程体验。 按照顺序处理这些教程，可以了解创建 WCF 应用程序所需的步骤。 完成后，你将有一个运行的 WCF 服务和一个调用服务的 WCF 客户端。

本教程假定你使用 Visual Studio 作为开发环境。 如果使用的是其他开发环境，请忽略特定于 Visual Studio 的说明。

有关可以下载和运行的示例 WCF 应用程序，请参阅 [Windows Communication Foundation 示例](samples/index.md)。 有关示例的简介，请参阅 [入门示例](samples/getting-started-sample.md)。

有关创建服务和客户端的更深入的信息，请参阅 [基本 WCF 编程](basic-wcf-programming.md)。

## <a name="wcf-tutorials"></a>WCF 教程

前三个教程介绍如何定义 WCF 服务协定，如何实现它，以及如何托管它。 在控制台应用程序中创建的服务是自承载的。 你还可以在 Microsoft Internet Information Services (IIS) 上承载服务。 有关详细信息，请参阅 [如何：在 IIS 中承载 WCF 服务](feature-details/how-to-host-a-wcf-service-in-iis.md)。 尽管在本教程中使用代码来配置服务，但也可以在 [配置文件中配置服务](configuring-services-using-configuration-files.md)。

- [教程：定义服务协定](how-to-define-a-wcf-service-contract.md)

    使用用户定义的接口创建 WCF 协定。 此协定定义服务公开的功能。

- [教程：实现服务协定](how-to-implement-a-wcf-contract.md)

    定义协定后，必须使用服务类实现该协定。

- [教程：托管并运行基本服务](how-to-host-and-run-a-basic-wcf-service.md)

    为服务配置终结点，并将服务托管在控制台应用程序中。 若要使服务变为活动状态，必须在运行时环境中对其进行配置并将其托管。 此运行时环境创建服务并控制其上下文和生存期。

接下来的两个教程介绍如何创建、配置和使用客户端应用程序来调用服务公开的操作。 服务发布定义客户端应用程序与服务进行通信所需信息的元数据。 Visual Studio 会自动执行访问此元数据的过程，并使用它来构造服务的客户端应用程序。 如果你决定不使用 Visual Studio，则可以改为使用 " [) 的元数据实用工具" 工具 (*Svcutil.exe*](servicemodel-metadata-utility-tool-svcutil-exe.md) 。

- [教程：创建客户端](how-to-create-a-wcf-client.md)

    检索用于从 WCF 服务创建 WCF 客户端代理的元数据。 您可以使用 Visual Studio 来检索元数据，以添加服务引用，也可以使用 "服务元数据实用工具"。 指定客户端用于访问服务的终结点。

- [教程：使用客户端](how-to-use-a-wcf-client.md)

    使用 WCF 客户端代理调用服务操作。

## <a name="reference"></a>参考

- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>

## <a name="see-also"></a>请参阅

- [概念概述](conceptual-overview.md)
- [文档指南](guide-to-the-documentation.md)
- [什么是 Windows Communication Foundation](whats-wcf.md)
- [WCF 功能详细信息](feature-details/index.md)
- [基本编程生命周期](basic-programming-lifecycle.md)
- [构建客户端](building-clients.md)
- [基本 WCF 编程](basic-wcf-programming.md)
- [如何：创建双工协定](feature-details/how-to-create-a-duplex-contract.md)
- [如何：使用双工协定访问服务](feature-details/how-to-access-services-with-a-duplex-contract.md)
- [ ( 2 # A0) ](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [如何：使用 Svcutil.exe 下载元数据文档](feature-details/how-to-use-svcutil-exe-to-download-metadata-documents.md)
- [如何：使用配置文件发布服务的元数据](feature-details/how-to-publish-metadata-for-a-service-using-a-configuration-file.md)
- [使用绑定配置服务和客户端](using-bindings-to-configure-services-and-clients.md)
- [入门示例](samples/getting-started-sample.md)
- [Windows Communication Foundation 示例](samples/index.md)
- [自承载](samples/self-host.md)
