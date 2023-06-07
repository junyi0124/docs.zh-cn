---
title: 协议缓冲区 - 适用于 WCF 开发人员的 gRPC
description: 用于 gRPC 网络的协议缓冲区线格式简介。
ms.date: 09/09/2019
ms.openlocfilehash: 35319d299a8bc2866a87954b3e54bfda9314ffe8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147927"
---
# <a name="protocol-buffers"></a>协议缓冲区

gRPC 服务以*协议缓冲区 （Protobuf） 消息*发送和接收数据，类似于 Windows 通信基础 （WCF） 中的数据协定。 Protobuf 是一种用于计算机读取和写入的结构化数据序列化的有效方法，无需产生 XML 或 JSON 等人类可读格式的开销。

本章介绍 Protobuf 的工作原理，以及如何定义您自己的 Protobuf 消息。

## <a name="how-protobuf-works"></a>普罗托布夫的工作原理

大多数 .NET 对象序列化技术（包括 WCF 的数据协定）都使用反射来分析运行时的对象结构。 相比之下，大多数 Protobuf 库要求您在`.proto`文件中使用专用语言（*协议缓冲区语言*）来预先定义结构。 然后，编译器使用此文件为任何受支持的平台生成代码。 支持的平台包括 .NET、Java、C/C++、JavaScript 等。

Protobuf 编译器`protoc`（） 由 Google 维护，尽管有替代实现可用。 生成的代码高效且优化，可快速序列化和数据反序列化。

Protobuf 线格式是二进制编码。 它使用一些巧妙的技巧来最小化用于表示消息的字节数。 使用 Protobuf 不需要了解二进制编码格式。 但是，如果你有兴趣，你可以了解更多关于它[的协议缓冲区网站](https://developers.google.com/protocol-buffers/docs/encoding)。

>[!div class="step-by-step"]
>[上一个](why-grpc.md)
>[下一个](protobuf-messages.md)
