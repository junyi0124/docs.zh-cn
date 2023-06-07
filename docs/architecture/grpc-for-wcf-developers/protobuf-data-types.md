---
title: Protobuf 标量数据类型-WCF 开发人员 gRPC
description: 了解 .NET Core 中的 Protobuf 和 gRPC 支持的基本数据类型和熟知数据类型。
ms.date: 09/09/2019
ms.openlocfilehash: 5447067b953d257258950d020636e0b38245e20d
ms.sourcegitcommit: 665f8fc55258356f4d2f4a6585b750c974b26675
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2020
ms.locfileid: "91573639"
---
# <a name="protobuf-scalar-data-types"></a>Protobuf 标量数据类型

协议缓冲区 (Protobuf) 支持一系列本机标量值类型。 下表列出了全部本机标量值类型及其等效 C# 类型：

| Protobuf 类型 | C# 类型      | 说明 |
| ------------- | ------------ | ----- |
| `double`      | `double`     |       |
| `float`       | `float`      |       |
| `int32`       | `int`        | 1     |
| `int64`       | `long`       | 1     |
| `uint32`      | `uint`       |       |
| `uint64`      | `ulong`      |       |
| `sint32`      | `int`        | 1     |
| `sint64`      | `long`       | 1     |
| `fixed32`     | `uint`       | 2     |
| `fixed64`     | `ulong`      | 2     |
| `sfixed32`    | `int`        | 2     |
| `sfixed64`    | `long`       | 2     |
| `bool`        | `bool`       |       |
| `string`      | `string`     | 3     |
| `bytes`       | `ByteString` | 4     |

说明：

1. 使用 `int32` 带符号值时，和的标准编码 `int64` 会效率低下。 如果你的字段可能包含负数，请改用 `sint32` 或 `sint64` 。 这些类型分别映射到 c # `int` 和 `long` 类型。
2. 无论 `fixed` 值为多少，字段始终使用相同的字节数。 此行为可以更快地对较大的值进行序列化和反序列化。
3. Protobuf 字符串是 UTF-8 (或7位 ASCII) 编码。 编码长度不能大于 2<sup>32</sup>。
4. Protobuf 运行时提供与 `ByteString` c # 数组轻松映射的类型 `byte[]` 。

## <a name="other-net-primitive-types"></a>其他 .NET 基元类型

### <a name="dates-and-times"></a>日期和时间

本机标量类型不提供日期和时间值，等效于 c # 的 <xref:System.DateTimeOffset> 、 <xref:System.DateTime> 和 <xref:System.TimeSpan> 。 你可以通过使用 Google 的某些 "知名类型" 扩展来指定这些类型。 这些扩展为受支持平台中的复杂字段类型提供代码生成和运行时支持。

下表显示日期和时间类型：

| C# 类型 | Protobuf 已知类型 |
| ------- | ------------------------ |
| `DateTimeOffset` | `google.protobuf.Timestamp` |
| `DateTime` | `google.protobuf.Timestamp` |
| `TimeSpan` | `google.protobuf.Duration` |

```protobuf  
syntax = "proto3"

import "google/protobuf/duration.proto";  
import "google/protobuf/timestamp.proto";

message Meeting {

    string subject = 1;
    google.protobuf.Timestamp time = 2;
    google.protobuf.Duration duration = 3;

}  
```

C# 类中生成的属性不是 .NET 日期和时间类型。 属性使用 `Google.Protobuf.WellKnownTypes` 命名空间中的 `Timestamp` 和 `Duration` 类。 这些类提供在 `DateTimeOffset`、`DateTime` 和 `TimeSpan` 之间进行转换的方法。

```csharp
// Create Timestamp and Duration from .NET DateTimeOffset and TimeSpan
var meeting = new Meeting
{
    Time = Timestamp.FromDateTimeOffset(meetingTime), // also FromDateTime()
    Duration = Duration.FromTimeSpan(meetingLength)
};

// Convert Timestamp and Duration to .NET DateTimeOffset and TimeSpan
DateTimeOffset time = meeting.Time.ToDateTimeOffset();
TimeSpan? duration = meeting.Duration?.ToTimeSpan();
```

> [!NOTE]
> `Timestamp` 类型适用于 UTC 时间。 `DateTimeOffset` 值的偏移量始终为零，并且 `DateTime.Kind` 属性始终为 `DateTimeKind.Utc`。

### <a name="systemguid"></a>System.Guid

Protobuf 不直接支持 <xref:System.Guid> `UUID` 在其他平台上称为的类型。 它没有已知的类型。

最佳方法是 `Guid` `string` 使用标准十六进制格式将值作为字段处理 `8-4-4-4-12` (例如， `45a9fda3-bd01-47a9-8460-c1cd7484b0b3`) 。 所有语言和平台都可以分析该格式。

不要对 `bytes` 值使用字段 `Guid` 。 当 Protobuf 与其他平台（如 Java）交互时， *endian* ([维基百科定义](https://en.wikipedia.org/wiki/Endianness)) 的问题可能导致不稳定的行为。

### <a name="nullable-types"></a>可为 null 的类型

C# 的 Protobuf 代码生成使用本机类型，如 `int` 表示 `int32`。 因此，这些值始终包括在内且不能为 null。

对于需要显式 null 的值（例如 `int?` ，在 c # 代码中使用），Protobuf 的 "知名类型" 包括编译为可为 null 的 c # 类型的包装。 若要使用它们，请导入 `wrappers.proto` 到 `.proto` 文件中，如下所示：

```protobuf  
syntax = "proto3"

import "google/protobuf/wrappers.proto"

message Person {

    ...
    google.protobuf.Int32Value age = 5;

}
```

Protobuf 将使用简单 `T?` (例如， `int?`) 生成的消息属性。

下表完整列出了包装器类型以及它们的等效 C# 类型：

| C# 类型   | 已知类型包装器       |
| --------- | ----------------------------- |
| `double?` | `google.protobuf.DoubleValue` |
| `float?`  | `google.protobuf.FloatValue`  |
| `int?`    | `google.protobuf.Int32Value`  |
| `long?`   | `google.protobuf.Int64Value`  |
| `uint?`   | `google.protobuf.UInt32Value` |
| `ulong?`  | `google.protobuf.UInt64Value` |

已知类型 `Timestamp` 和 `Duration` 以 .net 形式表示为类。 在 c # 8 和更高版本中，可以使用可以为 null 的引用类型。 但在转换为或时，必须检查这些类型的属性是否为 null `DateTimeOffset` `TimeSpan` 。

## <a name="decimals"></a>小数

Protobuf 本身不支持 .NET `decimal` 类型，只支持 `double` 和 `float`。 Protobuf 项目中有一项关于将标准 `Decimal` 类型添加到已知类型的可能，其中包含对支持它的语言和框架的平台支持。 尚未实现任何内容。

可以创建消息定义来表示 `decimal` 将在 .net 客户端和服务器之间进行安全序列化的类型。 但其他平台上的开发人员必须了解所使用的格式，并能够实现自己对其的处理。

### <a name="creating-a-custom-decimal-type-for-protobuf"></a>为 Protobuf 创建自定义 decimal 类型

简单实现可能与 `Money` 某些 Google api 使用的非标准类型相似，无需 `currency` 字段。

```protobuf
package CustomTypes;

// Example: 12345.6789 -> { units = 12345, nanos = 678900000 }
message DecimalValue {

    // Whole units part of the amount
    int64 units = 1;

    // Nano units of the amount (10^-9)
    // Must be same sign as units
    sfixed32 nanos = 2;
}
```

`nanos` 字段表示从 `0.999_999_999` 到 `-0.999_999_999` 的值。 例如，`decimal` 值 `1.5m` 将表示为 `{ units = 1, nanos = 500_000_000 }`。 这就是此示例中的 `nanos` 字段使用 `sfixed32` 类型的原因：对于较大的值，其编码效率比 `int32` 更高。 如果 `units` 字段为负，则 `nanos` 字段也应为负。

> [!NOTE]
> 还有多个其他算法可将 `decimal` 值编码为字节字符串，但此消息比其他任何字符串都易于理解。 不同平台上的值不受字节序影响。

此类型与 BCL `decimal` 类型之间的转换可能会在 C# 中实现，如下所示：

```csharp
namespace CustomTypes
{
    public partial class DecimalValue
    {
        private const decimal NanoFactor = 1_000_000_000;
        public DecimalValue(long units, int nanos)
        {
            Units = units;
            Nanos = nanos;
        }

        public long Units { get; }
        public int Nanos { get; }

        public static implicit operator decimal(CustomTypes.DecimalValue grpcDecimal)
        {
            return grpcDecimal.Units + grpcDecimal.Nanos / NanoFactor;
        }

        public static implicit operator CustomTypes.DecimalValue(decimal value)
        {
            var units = decimal.ToInt64(value);
            var nanos = decimal.ToInt32((value - units) * NanoFactor);
            return new CustomTypes.DecimalValue(units, nanos);
        }
    }
}
```

> [!IMPORTANT]
> 无论何时使用此类自定义消息类型，都 *必须* 在中记录注释 `.proto` 。 然后，其他开发人员可在其自己的语言或框架中实现与等效类型的转换。

>[!div class="step-by-step"]
>[上一页](protobuf-messages.md)
>[下一页](protobuf-nested-types.md)
