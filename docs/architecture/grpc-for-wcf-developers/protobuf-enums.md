---
title: 原数枚 - gRPC 面向 WCF 开发人员
description: 了解如何在 Protobuf 中声明和使用枚举。
ms.date: 09/09/2019
ms.openlocfilehash: 2177f568a671fa0e651625c6e025ac70c243feb5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148070"
---
# <a name="protobuf-enumerations"></a>Protobuf 枚举

Protobuf 支持枚举类型。 您在上一节中看到了此支持，其中使用枚举来确定`Oneof`字段的类型。 您可以定义自己的枚举类型，Protobuf 将将它们编译为 C# 枚举类型。

由于您可以将 Protobuf 用于各种语言，因此枚举的命名约定与 C# 约定不同。 但是，代码生成器将名称转换为传统的 C# 大小写。 如果字段名称的 Pascal 大小写以枚举名称开头，则将其删除。

例如，在下面的 Protobuf 枚举中，字段用 预缀为`ACCOUNT_STATUS`。 此前缀等效于 Pascal 大小写枚举名称`AccountStatus`。

```protobuf
enum AccountStatus {
  ACCOUNT_STATUS_UNKNOWN = 0;
  ACCOUNT_STATUS_PENDING = 1;
  ACCOUNT_STATUS_ACTIVE = 2;
  ACCOUNT_STATUS_SUSPENDED = 3;
  ACCOUNT_STATUS_CLOSED = 4;
}
```

生成器创建等效于以下代码的 C# 枚举：

```csharp
public enum AccountStatus
{
    Unknown = 0,
    Pending = 1,
    Active = 2,
    Suspended = 3,
    Closed = 4
}
```

Protobuf 枚举定义*必须*具有零常量作为第一个字段。 与 C# 中一样，您可以声明具有相同值的多个字段。 但是，您必须使用枚举中`allow_alias`的选项显式启用此选项：

```protobuf
enum AccountStatus {
  option allow_alias = true;
  ACCOUNT_STATUS_UNKNOWN = 0;
  ACCOUNT_STATUS_PENDING = 1;
  ACCOUNT_STATUS_ACTIVE = 2;
  ACCOUNT_STATUS_SUSPENDED = 3;
  ACCOUNT_STATUS_CLOSED = 4;
  ACCOUNT_STATUS_CANCELLED = 4;
}
```

您可以在`.proto`文件中的顶层声明枚举，也可以嵌套在消息定义中。 嵌套枚举（类似于嵌套消息）将在生成的消息类中的`.Types`静态类中声明。

无法将[[Flags]](xref:System.FlagsAttribute)属性应用于 Protobuf 生成的枚举，并且 Protobuf 不理解位级枚举组合。 查看以下示例：

```protobuf
enum Region {
  REGION_NONE = 0;
  REGION_NORTH_AMERICA = 1;
  REGION_SOUTH_AMERICA = 2;
  REGION_EMEA = 4;
  REGION_APAC = 8;
}

message Product {
  Region available_in = 1;
}
```

如果设置为`product.AvailableIn``Region.NorthAmerica | Region.SouthAmerica`，则序列化为整数值`3`。 当客户端或服务器尝试取消序列化该值时，它不会在 的`3`枚举定义中找到匹配项。 结果将为`Region.None`。

在 Protobuf 中使用多个枚举值的最佳方法是使用枚举类型的`repeated`字段。

>[!div class="step-by-step"]
>[上一个](protobuf-any-oneof.md)
>[下一个](protobuf-maps.md)
