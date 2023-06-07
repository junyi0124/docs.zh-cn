---
title: .NET Framework 中的网络跟踪
description: 了解 .NET Framework 中的网络跟踪，它提供有关托管应用程序的方法调用和网络流量的信息。
ms.date: 03/30/2017
helpviewer_keywords:
- debugging [.NET Framework], network tracing
- methods, network tracing
- Networking
- trace enabled debugging
- network tracing, about network tracing
- network tracing
- network, network tracing
- traffic tracing
- tracing [.NET Framework], network
- deploying applications [.NET Framework], network traffic
- capturing network traffic
- Internet, network tracing
- Network Resources
- output, network tracing
- method invocations
ms.assetid: e993b7c3-087f-45d8-9c02-9dded936d804
ms.openlocfilehash: c3c710d99c9597120b0c4d9674439a27c3bedfcc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261678"
---
# <a name="network-tracing-in-the-net-framework"></a>.NET Framework 中的网络跟踪

.NET Framework 中的网络跟踪允许访问有关方法调用的信息，以及有关托管应用程序所生成的网络流量的信息。 此功能可用于调试正在开发的应用程序，也可用于分析已部署的应用程序。 可以自定义网络跟踪所提供的输出，以支持在开发时和在生产环境中的不同使用方案。  
  
 若要启用 .NET Framework 中的网络跟踪，你必须为跟踪输出选择一个目的地，并将网络跟踪配置设置添加到应用程序配置文件或计算机配置文件。 有关配置文件及其使用方法的说明，请参阅[配置文件](../configure-apps/index.md)。 有关如何启用网络跟踪的信息，请参阅[启用网络跟踪](enabling-network-tracing.md)。 有关需要添加到配置文件的设置的信息，请参阅[如何：配置网络跟踪](how-to-configure-network-tracing.md)。  
  
 启用跟踪后，可捕获由 System.Net 类输出的跟踪信息。 可生成跟踪信息的网络类成员包括其 .NET Framework 类库文档“备注”部分中的以下注释：  
  
> [!NOTE]
> 当你在应用程序中启用网络跟踪后，此成员将输出跟踪信息。 有关详细信息，请参阅“网络跟踪”。  
  
## <a name="see-also"></a>请参阅

- [启用网络跟踪](enabling-network-tracing.md)
- [如何：配置网络跟踪](how-to-configure-network-tracing.md)
- [解释网络跟踪](interpreting-network-tracing.md)
- [跟踪应用程序和在应用程序中插入检测点](../debug-trace-profile/tracing-and-instrumenting-applications.md)
