---
title: 在联合方案中混合信任协议
ms.date: 03/30/2017
ms.assetid: d7b5fee9-2246-4b09-b8d7-9e63cb817279
ms.openlocfilehash: 5ce178c0b2c83469a26993ce6db2d6c87815543b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248170"
---
# <a name="mixing-trust-protocols-in-federated-scenarios"></a>在联合方案中混合信任协议

联合客户端与服务进行通信时，有些服务的信任版本与安全令牌服务 (STS) 的信任版本不同。 服务 WSDL 包含的 `RequestSecurityTokenTemplate` 断言可能具有与 STS 不同版本的 WS-Trust 元素。 在这种情况下，Windows Communication Foundation (WCF) 客户端会转换从接收的 WS-Trust 元素， `RequestSecurityTokenTemplate` 以便与 STS 信任版本相匹配。 WCF 只处理标准绑定的不匹配信任版本。 WCF 识别的所有标准算法参数均为标准绑定的一部分。 本主题介绍了在服务与 STS 之间具有各种信任设置的 WCF 行为。  
  
## <a name="rp-feb-2005-and-sts-feb-2005"></a>RP Feb 2005 和 STS Feb 2005  

 依赖方 (RP) 的 WSDL 在 `RequestSecurityTokenTemplate` 节中包含以下元素：  
  
- `CanonicalizationAlgorithm`  
  
- `EncryptionAlgorithm`  
  
- `EncryptWith`  
  
- `SignWith`  
  
- `KeySize`  
  
- `KeyType`  
  
 客户端配置文件包含一个参数列表。  
  
 WCF 无法区分客户端和服务参数;它添加所有参数，并以 `RequestSecurityTokenTemplate` (RST) 发送它们。  
  
## <a name="rp-trust-13-and-sts-trust-13"></a>RP Trust 1.3 和 STS Trust 1.3  

 RP 的 WSDL 在 `RequestSecurityTokenTemplate` 节中包含以下元素：  
  
- `CanonicalizationAlgorithm`  
  
- `EncryptionAlgorithm`  
  
- `EncryptWith`  
  
- `SignWith`  
  
- `KeySize`  
  
- `KeyType`  
  
- `KeyWrapAlgorithm`  
  
 客户端配置文件包含一个 `secondaryParameters` 元素，该元素包装 RP 所指定的参数。  
  
 `EncryptionAlgorithm` `CanonicalizationAlgorithm` `KeyWrapAlgorithm` 如果元素存在于元素内，WCF 将从 RST 的顶级元素中移除和元素 `SecondaryParameters` 。 WCF 将 `SecondaryParameters` 元素追加到未修改的传出 RST。  
  
## <a name="rp-trust-feb-2005-and-sts-trust-13"></a>RP Trust Feb 2005 和 STS Trust 1.3  

 RP 的 WSDL 在 `RequestSecurityTokenTemplate` 节中包含以下元素：  
  
- `CanonicalizationAlgorithm`  
  
- `EncryptionAlgorithm`  
  
- `EncryptWith`  
  
- `SignWith`  
  
- `KeySize`  
  
- `KeyType`  
  
 客户端配置文件包含一个参数列表。  
  
 在客户端配置文件中，WCF 无法区分服务参数和客户端参数。 因此，WCF 会将所有参数转换为信任版本1.3 命名空间。  
  
 WCF 处理 `KeyType` 、 `KeySize` 和元素， `TokenType` 如下所示：  
  
- 下载 WSDL，创建绑定，并从 RP 参数分配 `KeyType`、`KeySize` 和 `TokenType`。 随后会生成客户端配置文件。  
  
- 客户端现在可以更改配置文件中的任何参数。  
  
- 在运行时，WCF 会将指定的所有参数都复制到 `AdditionalTokenParameters` 客户端配置文件的节中（和除外）， `KeyType` `KeySize` `TokenType` 因为这些参数在配置文件生成过程中进行考虑。  
  
## <a name="rp-trust-13-and-sts-trust-feb-2005"></a>RP Trust 1.3 和 STS Trust Feb 2005  

 RP 的 WSDL 在 `RequestSecurityTokenTemplate` 节中包含以下元素：  
  
- `CanonicalizationAlgorithm`  
  
- `EncryptionAlgorithm`  
  
- `EncryptWith`  
  
- `SignWith`  
  
- `KeySize`  
  
- `KeyType`  
  
- `KeyWrapAlgorithm`  
  
 客户端配置文件包含一个 `secondaryParamters` 元素，该元素包装 RP 所指定的参数。  
  
 WCF 将部分中指定的所有参数复制 `SecondaryParameters` 到顶层 RST 元素，但不会将其转换为 2005 WS-Trust 命名空间。
