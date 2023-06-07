---
ms.openlocfilehash: 7c0227980aa5d90f3788783088bcd7cd9509ed66
ms.sourcegitcommit: 261e0c98a111357692b3b63c596edf0cacf72991
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90770943"
---
### <a name="wcf-msmqsecurehashalgorithm-default-value-is-now-sha256"></a>WCF MsmqSecureHashAlgorithm 现在的默认值为 SHA256

#### <a name="details"></a>详细信息

自 .NET Framework 4.7.1 起，Msmq 消息 WCF 中的默认消息签名算法为 SHA256。 在 .NET Framework 4.7 和更低版本中，默认消息签名算法是 SHA1。

#### <a name="suggestion"></a>建议

如果在 .NET Framework 4.7.1 或更高版本中遇到此更改的兼容性问题，可以将以下行添加到 app.config 文件的 `<runtime>` 部分，以选择退出更改：

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value=&quot;Switch.System.ServiceModel.UseSha1InMsmqEncryptionAlgorithm=true&quot; />
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
