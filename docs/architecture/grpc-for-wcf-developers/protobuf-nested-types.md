---
title: Protobuf 嵌套类型-WCF 开发人员 gRPC
description: 了解 Protobuf 和 gRPC 中的嵌套消息类型及其生成方式C#。
ms.date: 09/09/2019
ms.openlocfilehash: 7b9a331336ebe1ca7bc75fdd164b7b88ae4f9db2
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77542841"
---
# <a name="protobuf-nested-types"></a>Protobuf 嵌套类型

正如C#允许在其他类中声明类一样，通过协议缓冲区（Protobuf）可以在其他消息中嵌套消息定义。 下面的示例演示如何创建嵌套消息类型：

```protobuf
message Outer {
    message Inner {
        string text = 1;
    }
    Inner inner = 1;
}
```

在生成C#的代码中，将在 `HelloRequest` 类中的嵌套静态 `Types` 类中声明 `Inner` 类型：

```csharp
var inner = new Outer.Types.Inner { Text = "Hello" };
```

>[!div class="step-by-step"]
>[上一页](protobuf-data-types.md)
>[下一页](protobuf-repeated.md)
