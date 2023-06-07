---
ms.openlocfilehash: c967a5b09b5e9ffeee7bff046f0c96469bc7fb02
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615604"
---
### <a name="clickonce-supports-sha-256-on-40-targeted-apps"></a>ClickOnce 支持面向 4.0 的应用上的 SHA-256

#### <a name="details"></a>详细信息

以前，如果 ClickOnce 应用具有使用 SHA-256 签名的证书，即使应用面向 4.0 版本，也需要 .NET Framework 4.5 或更高版本。 现在，即使使用 SHA-256 签名，面向 .NET Framework 4.0 的 ClickOnce 应用也可在 .NET Framework 4.0 上运行。

#### <a name="suggestion"></a>建议

此更改删除了该依赖项，允许将 SHA-256 证书用于为面向 .NET Framework 4 或更早版本的 ClickOnce 应用签名。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |
