---
title: 改进的强命名
description: .NET Framework 中的程序集的传统强名称签名有一些限制。 了解改进的强命名。
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies
- strong naming [.NET Framework], enhanced
ms.assetid: 6cf17a82-62a1-4f6d-8d5a-d7d06dec2bb5
ms.openlocfilehash: b9c690c77dafc247f7282c3f56384481efe53399
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731522"
---
# <a name="enhanced-strong-naming"></a>改进的强命名

强名称签名是 .NET Framework 中用于识别程序集的识别机制。 它是一个公钥数字签名，通常用于验证发信方（签名方）发送给收信方（验证方）的数据的完整性。 此签名是程序集的唯一标识，可确保对程序集的引用清楚明确。 在生成过程中对程序集进行签名，然后在加载程序集时对其进行验证。  
  
 强名称签名有助于防止恶意方篡改程序集，然后使用原始签名方的密钥对程序集进行重新签名。 但是强名称密钥不包含任何有关发布服务器的可靠信息，也不包含任何证书层次结构。 强名称签名不保证为程序集签名的人员的可信度，也不指示该人员是否为密钥的合法所有者；它仅指示密钥所有者对程序集进行了签名。 因此，不建议使用强名称签名作为判断是否应信任第三方代码的安全验证程序。 建议使用 Microsoft Authenticode 对代码进行身份验证。  
  
## <a name="limitations-of-conventional-strong-names"></a>传统的强名称的限制  

 在早于 .NET Framework 4.5 的版本中使用强命名技术有以下缺点：  
  
- 密钥会不断受到攻击，并且技术和硬件的改进让根据公钥推断私钥变得更加容易。 抵御攻击需要使用更大的密钥。 早于 .NET Framework 4.5 的 .NET Framework 版本可以使用任意大小的密钥进行签名（默认大小为 1024 位），但是使用新密钥对程序集签名会破坏引用程序集旧标识的所有二进制文件。 因此，如果想要保持兼容性，就很难升级签名密钥的大小。  
  
- 强名称签名仅支持 SHA-1 算法。 人们最近发现 SHA-1 不足以保证哈希应用程序的安全。 因此需要使用更强的算法（SHA-256 或更高）。 SHA-1 可能不再与 FIPS 兼容，这样的话选择只使用与 FIPS 兼容的软件和算法的用户可能会遇到问题。  
  
## <a name="advantages-of-enhanced-strong-names"></a>改进的强名称的优点  

 增强的强名称最主要的优势是与预先存在的强名称兼容，并且能够声明一个标识等效于另一个标识：  
  
- 具有现有已签名程序集的开发人员可以将其标识迁移到 SHA-2 算法，同时兼容引用旧标识的程序集。  
  
- 创建新的程序集并且不考虑已有的强名称签名的开发人员可以使用安全性更高的 SHA-2 算法，并按惯用的方式对程序集签名。  
  
## <a name="use-enhanced-strong-names"></a>使用改进的强名称  

 强名称密钥包含签名密钥和标识密钥。 使用签名密钥对程序集进行签名，使用标识密钥标识该程序集。 在 .NET Framework 4.5 之前，这两个密钥相同。 从 .NET Framework 4.5 开始，标识密钥与早期 .NET Framework 版本中的相同，但是签名密钥使用了更强的哈希算法进行了改进。 此外，使用标识密钥对签名密钥进行签名以创建副署。  
  
 通过 <xref:System.Reflection.AssemblySignatureKeyAttribute> 属性，程序集元数据可将已有公钥用于程序集标识，让旧程序集引用仍然有效。  <xref:System.Reflection.AssemblySignatureKeyAttribute> 使用副署来确保新签名密钥的所有者同时也是旧标识密钥的所有者。  
  
### <a name="sign-with-sha-2-without-key-migration"></a>使用 SHA-2 签名，不进行密钥迁移  

 在命令提示符中运行以下命令，在不迁移强名称签名的情况下对程序集签名：  
  
1. 生成新的标识密钥（如有必要）。  
  
    ```console  
    sn -k IdentityKey.snk  
    ```  
  
2. 提取标识公钥，并指定使用此密钥进行签名时，须使用 SHA-2 算法。  
  
    ```console  
    sn -p IdentityKey.snk IdentityPubKey.snk sha256  
    ```  
  
3. 使用标识公钥文件对程序集进行延迟签名。  
  
    ```console  
    csc MyAssembly.cs /keyfile:IdentityPubKey.snk /delaySign+  
    ```  
  
4. 使用完整标识密钥对对程序集进行重新签名。  
  
    ```console  
    sn -Ra MyAssembly.exe IdentityKey.snk  
    ```  
  
### <a name="sign-with-sha-2-with-key-migration"></a>使用 SHA-2 签名，并进行密钥迁移  

 在命令提示符中运行以下命令，在已迁移强名称签名的情况下对程序集签名。  
  
1. 生成标识密钥和签名密钥对（如有必要）。  
  
    ```console  
    sn -k IdentityKey.snk  
    sn -k SignatureKey.snk  
    ```  
  
2. 提取签名公钥，并指定使用此密钥进行签名时，必须使用 SHA-2 算法。  
  
    ```console  
    sn -p SignatureKey.snk SignaturePubKey.snk sha256  
    ```  
  
3. 提取标识公钥，该公钥确定用于生成副署的哈希算法。  
  
    ```console  
    sn -p IdentityKey.snk IdentityPubKey.snk  
    ```  
  
4. 生成 <xref:System.Reflection.AssemblySignatureKeyAttribute> 属性的参数，并将该参数附加到程序集。  
  
    ```console  
    sn -a IdentityPubKey.snk IdentityKey.snk SignaturePubKey.snk  
    ```  

    这将生成类似下面的输出。

    ```output
    Information for key migration attribute.
    (System.Reflection.AssemblySignatureKeyAttribute):
    publicKey=
    002400000c80000094000000060200000024000052534131000400000100010005a3a81ac0a519
    d96244a9c589fc147c7d403e40ccf184fc290bdd06c7339389a76b738e255a2bce1d56c3e7e936
    e4fc87d45adc82ca94c716b50a65d39d373eea033919a613e4341c66863cb2dc622bcb541762b4
    3893434d219d1c43f07e9c83fada2aed400b9f6e44ff05e3ecde6c2827830b8f43f7ac8e3270a3
    4d153cdd

    counterSignature=
    e3cf7c211678c4d1a7b8fb20276c894ab74c29f0b5a34de4d61e63d4a997222f78cdcbfe4c91eb
    e1ddf9f3505a32edcb2a76f34df0450c4f61e376b70fa3cdeb7374b1b8e2078b121e2ee6e8c6a8
    ed661cc35621b4af53ac29c9e41738f199a81240e8fd478c887d1a30729d34e954a97cddce66e3
    ae5fec2c682e57b7442738
    ```

    然后可以将此输出转换为 AssemblySignatureKeyAttribute。

    ```csharp
    [assembly:System.Reflection.AssemblySignatureKeyAttribute(
    "002400000c80000094000000060200000024000052534131000400000100010005a3a81ac0a519d96244a9c589fc147c7d403e40ccf184fc290bdd06c7339389a76b738e255a2bce1d56c3e7e936e4fc87d45adc82ca94c716b50a65d39d373eea033919a613e4341c66863cb2dc622bcb541762b43893434d219d1c43f07e9c83fada2aed400b9f6e44ff05e3ecde6c2827830b8f43f7ac8e3270a34d153cdd",
    "e3cf7c211678c4d1a7b8fb20276c894ab74c29f0b5a34de4d61e63d4a997222f78cdcbfe4c91ebe1ddf9f3505a32edcb2a76f34df0450c4f61e376b70fa3cdeb7374b1b8e2078b121e2ee6e8c6a8ed661cc35621b4af53ac29c9e41738f199a81240e8fd478c887d1a30729d34e954a97cddce66e3ae5fec2c682e57b7442738"
    )]
    ```
  
5. 使用标识公钥延迟对程序集签名。  
  
    ```console  
    csc MyAssembly.cs /keyfile:IdentityPubKey.snk /delaySign+  
    ```  
  
6. 使用签名密钥对对程序集进行完整签名。  
  
    ```console  
    sn -Ra MyAssembly.exe SignatureKey.snk  
    ```  
  
## <a name="see-also"></a>请参阅

- [创建和使用具有强名称的程序集](create-use-strong-named.md)
