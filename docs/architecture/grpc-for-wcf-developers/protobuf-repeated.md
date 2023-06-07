---
title: 列表和数组的重复字段-gRPC for WCF 开发人员
description: 了解 Protobuf 如何处理集合及其与 .NET 集合之间的关系。
ms.date: 09/09/2019
ms.openlocfilehash: 7320c76ddc58bcf5cd81150923e8cb635e510047
ms.sourcegitcommit: b9122d1af21898eaba81e990c70fef46fef74a8d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/26/2020
ms.locfileid: "88867498"
---
# <a name="repeated-fields-for-lists-and-arrays"></a>列表和数组的重复字段

使用 prefix 关键字在协议缓冲区 (Protobuf) 中指定列表 `repeated` 。 下面的示例演示如何创建列表：

```protobuf
message Person {
    // Other fields elided
    repeated string aliases = 8;
}
```

在生成的代码中， `repeated` 字段由类型的只读属性 [`Google.Protobuf.Collections.RepeatedField<T>`][repeated-field] （而不是任何内置 .net 集合类型）表示。 此类型实现所有标准 .NET 集合接口，例如 <xref:System.Collections.Generic.IList%601> 和 <xref:System.Collections.Generic.IEnumerable%601> 。 因此，您可以使用 LINQ 查询或轻松地将其转换为数组或列表。

`RepeatedField<T>`类型包括将列表序列化和反序列化为二进制线路格式所需的代码。

[repeated-field]: https://developers.google.cn/protocol-buffers/docs/reference/csharp/class/google/protobuf/collections/repeated-field-t-

>[!div class="step-by-step"]
>[上一页](protobuf-nested-types.md)
>[下一页](protobuf-reserved.md)
