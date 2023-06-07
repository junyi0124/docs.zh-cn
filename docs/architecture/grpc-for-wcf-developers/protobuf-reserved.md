---
title: Protobuf 保留字段-WCF 开发人员 gRPC
description: 了解有关跨版本兼容性的保留字段。
ms.date: 12/15/2020
ms.openlocfilehash: d82043d619c5b3b81091ae4ea381e4778c1cf6ba
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938515"
---
# <a name="protobuf-reserved-fields"></a>Protobuf 保留字段

协议缓冲区中的向后兼容性保证 (Protobuf) 依赖于字段编号始终表示相同的数据项。 如果从服务的新版本的消息中删除字段，则永远不应重复使用该字段号。 可以通过使用关键字强制执行此行为 `reserved` 。

如果 `displayName` `marketId` 已从前面定义的消息中删除了和字段 `Stock` ，则应该保留其字段编号，如以下示例中所示。

```protobuf
syntax "proto3";

message Stock {

    reserved 3, 4;
    int32 id = 1;
    string symbol = 2;

}
```

你还可以使用 `reserved` 关键字作为以后可能添加的字段的占位符。 您可以使用关键字将连续的字段编号表示为范围 `to` 。

```protobuf
syntax "proto3";

message Info {

    reserved 2, 9 to 11, 15;
    // ...
}
```

>[!div class="step-by-step"]
>[上一页](protobuf-repeated.md)
>[下一页](protobuf-any-oneof.md)
