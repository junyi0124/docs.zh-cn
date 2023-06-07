---
title: FIPS 相容性-.NET Core
description: 介绍 .NET Core 美国联邦信息处理标准（FIPS）符合性。
ms.date: 11/20/2019
author: Rick-Anderson
ms.author: riande
ms.openlocfilehash: 84d6edc6975af0484bb67999ad5891eaf4db057d
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77626953"
---
# <a name="net-core-federal-information-processing-standard-fips-compliance"></a>.NET Core 美国联邦信息处理标准（FIPS）符合性

美国联邦信息处理标准（FIPS）发布140-2 是美国政府标准，用于定义信息技术产品中加密模块的最低安全要求，如信息的第5131节中所定义技术管理改革行为1996。

.NET Core：

* 将加密基元调用传递到基础操作系统提供的标准模块。
* **不**强制在 .net Core 应用程序中使用 FIPS 批准的算法或密钥大小。

系统管理员负责为操作系统配置 FIPS 相容性。

如果为符合 FIPS 标准的环境编写代码，开发人员负责确保不会使用不合规的 FIPS 算法。

有关 FIPS 相容性的详细信息，请参阅以下文章：

* [Windows FIPS 相容性](/windows/security/threat-protection/fips-140-validation)
* [针对 FIPS 相容性配置 Windows](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing)
* [第8章：联邦标准和法规 Red Hat Enterprise Linux 7](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/chap-federal_standards_and_regulations)
