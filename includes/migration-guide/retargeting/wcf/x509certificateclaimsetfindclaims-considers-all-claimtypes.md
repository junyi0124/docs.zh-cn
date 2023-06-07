---
ms.openlocfilehash: abef47c64a7cfd99c5b50bf2100401a2d5fcc5c3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614355"
---
### <a name="x509certificateclaimsetfindclaims-considers-all-claimtypes"></a>X509CertificateClaimSet.FindClaims 考虑到所有 claimTypes

#### <a name="details"></a>详细信息

在面向 .NET Framework 4.6.1 的应用中，如果从 SAN 字段拥有多个 DNS 条目的证书初始化 X509 声明集，<xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims(System.String,System.String)?displayProperty=fullName> 方法会尝试将 claimType 自变量与所有 DNS 条目进行匹配。对于面向以前版本的 .NET Framework 的应用，<xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims(System.String,System.String)?displayProperty=fullName> 方法会尝试仅将 claimType 自变量与最后一个 DNS 条目进行匹配。

#### <a name="suggestion"></a>建议

此更改仅影响面向 .NET Framework 4.6.1 的应用程序。 在 [DisableMultipleDNSEntries](~/docs/framework/migration-guide/mitigation-x509certificateclaimset-findclaims-method.md#mitigation) 兼容性开关中，可能会禁用此更改（如果面向 4.6.1 之前的版本，将启用此更改）。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims(System.String,System.String)?displayProperty=nameWithType>
