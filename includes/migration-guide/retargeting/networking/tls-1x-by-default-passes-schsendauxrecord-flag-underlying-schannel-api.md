---
ms.openlocfilehash: 9fd4e570d9476f5ac4c310759113d72b354a3060
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606744"
---
### <a name="tls-1x-by-default-passes-the-sch_send_aux_record-flag-to-the-underlying-schannel-api"></a>默认情况下，TLS 1.x 将 SCH_SEND_AUX_RECORD 标记传递给基础 SCHANNEL API

#### <a name="details"></a>详细信息

使用 TLS 1.x 时，.NET Framework 依赖于基础 Windows SCHANNEL API。 从 .NET Framework 4.6 开始，[SCH_SEND_AUX_RECORD](/windows/win32/api/schannel/ns-schannel-schannel_cred) 标记默认传递给 SCHANNEL。 这会导致 SCHANNEL 将要加密的数据拆分为两个单独的记录，第一个为 1 个字节，第二个为 <em>n</em>-1 个字节。在极少数情况下，如果假定数据驻留在一个记录中，则客户端与现有服务器之间的通信会中断。

#### <a name="suggestion"></a>建议

如果此更改中断了与现有服务器的通信，则可以将以下开关添加到应用配置文件的 [`<runtime>`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) 部分中的 [`<AppContextSwitchOverrides>`](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素，禁止发送 [SCH_SEND_AUX_RECORD](/windows/win32/api/schannel/ns-schannel-schannel_cred) 标记并还原之前不将数据拆分为单独记录的行为：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Net.DontEnableSchSendAuxRecord=true" />
</runtime>
```

> [!IMPORTANT]
> 此设置仅用于后向兼容性。 否则不建议使用它。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.Security.SslStream?displayProperty=nameWithType>
- <xref:System.Net.ServicePointManager?displayProperty=nameWithType>
- <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.FtpWebRequest?displayProperty=nameWithType>
