---
ms.openlocfilehash: 7e42a294b151d07a6fdc61308cdf92df7a31a1d6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614351"
---
### <a name="deflatestream-uses-native-apis-for-decompression"></a>DeflateStream 使用本机 API 进行解压缩

#### <a name="details"></a>详细信息

从 .NET Framework 4.7.2 开始，`T:System.IO.Compression.DeflateStream` 类中解压缩的实现已更改为默认使用本机 Windows API。 通常情况下，这能大大地提高性能。 所有面向 .NET Framework 4.7.2 或更高版本的 .NET 应用程序均使用本机实现。此更改可能会导致某些行为差异，其中包括：

- 异常消息可能有所不同。 但是，引发的异常类型保持不变。
- 可能以不同的方式处理某些特殊情况（例如没有足够的内存完成操作）。
- 分析 gzip 标头存在一些已知差异（注意：仅影响用于解压缩的 `GZipStream` 集）：
- 分析无效标头时出现异常可能在不同的时间引发。
- 本机实现强制根据规范设置 gzip 标头（即 [FLG](http://www.zlib.org/rfc-gzip.html#header-trailer)）内的一些保留标记值，这可能导致其在忽略先前无效值的情况下引发异常。

#### <a name="suggestion"></a>建议

如果借助本机 API 解压缩对应用行为产生负面影响，则可通过将 `Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression` 开关添加到 app.config 文件的 `runtime` 部分并将其设置为 `true`，选择弃用此功能：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression=true" />
  </runtime>
</configuration>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| 版本 | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Compression.DeflateStream?displayProperty=nameWithType>
- <xref:System.IO.Compression.GZipStream?displayProperty=nameWithType>
