---
title: Windows Communication Foundation 体系结构
description: 了解 Windows Communication Foundation 体系结构的主要层，包括协定、服务运行时、消息传递和 & 托管的激活。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], architecture
- WCF [WCF], architecture
- architecture [WCF]
ms.assetid: a3bcb0a1-56ea-4ba6-9736-d260d90dade5
ms.openlocfilehash: 7cb03ddd7207fbea23577ff18e984e90779333bc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294880"
---
# <a name="windows-communication-foundation-architecture"></a>Windows Communication Foundation 体系结构

下图说明了 Windows Communication Foundation (WCF) 体系结构的主要层。  
  
## <a name="wcf-architecture"></a>WCF 体系结构  

 ![WCF 体系结构](./media/wcf-architecture.gif "WCF_Architecture")  
  
### <a name="contracts-and-descriptions"></a>协定和说明  

 协定定义消息系统的各个方面。 数据协定描述组成某一服务可创建或使用的每则消息的每个参数。 消息参数由 XML 架构定义语言 (XSD) 文档定义，这使得任何理解 XML 的系统均可处理该文档。 消息协定使用 SOAP 协议定义特定消息部分，当互操作性要求对消息的某些部分进行更精细的控制时，消息协定可实现这种控制。 服务协定指定服务的实际方法签名，并以支持的编程语言之一（例如 Visual Basic 或 Visual C#）作为接口进行分发。  
  
 策略和绑定规定与某一服务进行通信所需的条件。  例如，绑定必须（至少）指定所使用的传输（例如 HTTP 或 TCP）和编码。 策略包括安全要求和其他条件，必须满足这些要求和条件才能与服务进行通信。  
  
### <a name="service-runtime"></a>服务运行时  

 服务运行时层包含仅在服务实际运行期间发生的行为，即该服务的运行时行为。 遏制控制处理的消息数，如果对服务的需求增长到预设限制，该消息数则会发生变化。 错误行为指定服务出现内部错误时应采取的操作，例如控制传递给客户端的信息  (太多的信息会使恶意用户有权安装攻击。 ) 元数据行为控制如何以及是否可将元数据提供给外界。 实例行为指定可运行的服务实例的数目（例如，singleton 指定只能用单一实例来处理所有消息）。 通过事务行为，可以在失败时回滚已进行事务处理的操作。 调度行为用于控制 WCF 基础结构处理消息的方式。  
  
 通过扩展性功能可以自定义运行时进程。 例如，消息检查功能用于检查消息的各个部分，使用参数筛选功能可以根据作用于消息头的筛选器来执行预设操作。  
  
### <a name="messaging"></a>Messaging  

 消息传送层由 *通道* 组成。 通道是以某种方式对消息进行处理（例如通过对消息进行身份验证）的组件。 一组通道也称为 *通道堆栈*。 通道对消息和消息头进行操作。 这与服务运行时层不同，服务运行时层主要涉及对消息正文内容的处理。  
  
 有两种类型的通道：传输通道和协议通道。  
  
 传输通道读取和写入来自网络（或外部的某些其他通信点）的消息。 某些传输通道使用编码器来将消息（表示为 XML Infoset）转换为网络所使用的字节流的表示形式，或将字节流表示形式转换为消息。 传输通道的示例包括 HTTP、命名管道、TCP 和 MSMQ。 编码的示例包括 XML 和优化的二进制文件。  
  
 协议通道经常通过读取或写入消息的其他头的方式来实现消息处理协议。 此类协议的示例包括 WS-Security 和 WS-Reliability。  
  
 消息传递层说明数据的可能格式和交换模式。 WS-Security 是对在消息层启用安全性的 WS-Security 规范的实现。 通过 WS-Reliable Messaging 通道可以保证消息的传递。 编码器提供了大量的编码，可使用这些编码来满足消息的需要。 HTTP 通道指定应使用超文本传输协议来传递消息。 同理，TCP 通道指定 TCP 协议。 事务流通道控制已经过事务处理的消息模式。 通过命名管道通道可以进行进程间通信。 使用 MSMQ 通道可以与 MSMQ 应用程序进行互操作。  
  
### <a name="hosting-and-activation"></a>承载和激活  

 服务的最终形式为程序。 与其他程序类似，服务必须在可执行文件中运行。 这称为 *自承载* 服务。  
  
 服务还可以 *托管*，也可以在由外部代理管理的可执行文件中运行，例如 IIS 或 Windows 激活服务 () 。 已启用的 WCF 应用程序部署到运行 WAS 的计算机上时，它将自动激活。 还可通过可执行文件（.exe 文件）的形式来手动运行服务。 服务也可作为 Windows 服务自动运行。 COM + 组件还可以作为 WCF 服务托管。  
  
## <a name="see-also"></a>另请参阅

- [什么是 Windows Communication Foundation](whats-wcf.md)
- [Windows Communication Foundation 基础概念](fundamental-concepts.md)
