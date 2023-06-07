---
ms.openlocfilehash: 5c09bee92f679cd7e7a95cd23d5ce0ca9b57170c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614358"
---
### <a name="wcf-transport-security-supports-certificates-stored-using-cng"></a>WCF 传输安全性支持使用 CNG 存储的证书

#### <a name="details"></a>详细信息

从面向 .NET Framework 4.6.2 的应用起，WCF 传输安全性支持使用 Windows 加密库 (CNG) 存储的证书。 此支持仅限于将证书与指数长度不超过 32 位的公钥结合使用。 当应用程序面向 .NET Framework 4.6.2 时，此功能默认启用。在旧版 .NET framework 中，尝试将 X509 证书与 CSG 密钥存储提供程序结合使用会引发异常。

#### <a name="suggestion"></a>建议

应用如果面向 .NET Framework 4.6.1 和更早版本，但在 .NET Framework 4.6.2 上运行，则可通过将以下行添加到 app.config 或 web.config 文件的 `<runtime>` 部分来启用对 CNG 证书的支持：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableCngCertificates=false" />
</runtime>
```

也可以使用以下代码以编程方式完成此操作：

```csharp
private const string DisableCngCertificates = @"Switch.System.ServiceModel.DisableCngCertificate";

AppContext.SetSwitch(disableCngCertificates, false);
```

```vb
Const DisableCngCertificates As String = "Switch.System.ServiceModel.DisableCngCertificates"
AppContext.SetSwitch(disableCngCertificates, False)
```

请注意，鉴于有此更改，将不再执行任何依赖无法尝试使用 CNG 证书启动安全通信的异常处理代码。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6.2       |
| 类型    | 重定目标 |
