---
ms.openlocfilehash: 9f5f238e3d4222af1da3a1713e1b3e65de6e6f49
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "91024930"
---
### <a name="wcf-pipeconnectiongethashalgorithm-now-uses-sha256"></a>WCF PipeConnection.GetHashAlgorithm 现在使用 SHA256

#### <a name="details"></a>详细信息

从 .NET Framework 4.7.1 开始，Windows Communication Foundation 使用 SHA256 哈希为命名管道生成随机名称。 在 .NET Framework 4.7 和更早版本中，它使用 SHA1 哈希。

#### <a name="suggestion"></a>建议

如果在 .NET Framework 4.7.1 或更高版本中遇到此更改的兼容性问题，则可以通过将以下行添加到 app.config 文件的 `<runtime>` 部分选择弃用此更改：

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.ServiceModel.UseSha1InPipeConnectionGetHashAlgorithm=true" />
  </runtime>
</configuration>
```

| 名称    | 值   |
|:--------|:--------|
| 范围   | 次要   |
| Version | 4.7.1   |
| 类型    | 运行时 |

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
