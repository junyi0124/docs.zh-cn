---
title: 可靠会话
ms.date: 03/30/2017
f1_keywords:
- Windows Communication Foundation, sessions and instances
- WCF, sessions and instances
- sessions and instances [WCF]
helpviewer_keywords:
- instances [WCF]
- sessions [WCF]
ms.assetid: 143951b3-3aa0-4540-b4b7-d33e77e874a1
ms.openlocfilehash: 910ad952192243c6aa8a79417ad711d8c2a4ba2e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590535"
---
# <a name="reliable-sessions"></a>可靠会话

本部分介绍什么是 Windows Communication Foundation （WCF）可靠会话、其用途、使用方式、使用方式、使用的绑定配置以及最佳实践的指针。 下表汇总了有关本节中要点和相关主题的详细信息。

可靠会话 WCF 提供的功能可确保在终结点之间发送的消息在 SOAP 或传输中介之间传输，并且只传递一次，并根据发送的相同顺序进行传递。

若要对 WCF 应用程序使用可靠会话，请使用 WCF 中某个系统提供的绑定（默认情况下支持可靠会话）或作为选项，或创建你自己的支持会话的自定义绑定。

## <a name="in-this-section"></a>本节内容

[可靠会话概述](reliable-sessions-overview.md)介绍可靠会话、何时使用它们、支持可靠会话的不同绑定，以及它们的工作方式。

[如何：在可靠会话内交换消息](how-to-exchange-messages-within-a-reliable-session.md)介绍如何使用配置中指定的自定义绑定通过 HTTP 创建可靠会话。

[如何：在可靠会话内保护消息](how-to-secure-messages-within-reliable-sessions.md)介绍如何保护可靠会话。

[如何：使用 HTTPS 创建自定义可靠会话绑定](how-to-create-a-custom-reliable-session-binding-with-https.md)介绍如何通过 HTTPS 创建可靠会话。

[可靠会话的最佳做法](best-practices-for-reliable-sessions.md)介绍与使用可靠会话相关的一些最佳实践。

## <a name="reference"></a>参考

<xref:System.ServiceModel.ReliableSession>

## <a name="see-also"></a>请参阅

- [队列和可靠会话](queues-and-reliable-sessions.md)
- [会话、实例化和并发](sessions-instancing-and-concurrency.md)
