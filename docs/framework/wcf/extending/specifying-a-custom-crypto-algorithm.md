---
title: 指定自定义加密算法
ms.date: 03/30/2017
ms.assetid: d662a305-8e09-451d-9a59-b0f12b012f1d
ms.openlocfilehash: bdb7d45752be94c4c81e27161f57f765d64bd94a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293996"
---
# <a name="specifying-a-custom-crypto-algorithm"></a>指定自定义加密算法

WCF 允许指定在加密数据或计算数字签名时使用的自定义加密算法。 为此，请执行下列步骤：  
  
1. 从 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 中派生一个类  
  
2. 注册算法  
  
3. 配置与 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 派生类的绑定。  
  
## <a name="derive-a-class-from-securityalgorithmsuite"></a>从 SecurityAlgorithmSuite 中派生一个类  

 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 是一个抽象基类，可用于指定执行各种安全相关操作时使用的算法。 例如，计算数字签名的哈希值或加密消息。 以下代码揭示了如何从 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 中派生类：  
  
```csharp  
public class MyCustomAlgorithmSuite : SecurityAlgorithmSuite  
    {  
        public override string DefaultAsymmetricKeyWrapAlgorithm  
        {  
            get { return SecurityAlgorithms.RsaOaepKeyWrap; }  
        }  
  
        public override string DefaultAsymmetricSignatureAlgorithm  
        {  
            get { return SecurityAlgorithms.RsaSha1Signature; }  
        }  
  
        public override string DefaultCanonicalizationAlgorithm  
        {  
            get { return SecurityAlgorithms.ExclusiveC14n; ; }  
        }  
  
        public override string DefaultDigestAlgorithm  
        {  
            get { return SecurityAlgorithms.MyCustomHashAlgorithm; }  
        }  
  
        public override string DefaultEncryptionAlgorithm  
        {  
            get { return SecurityAlgorithms.Aes128Encryption; }  
        }  
  
        public override int DefaultEncryptionKeyDerivationLength  
        {  
            get { return 128; }  
        }  
  
        public override int DefaultSignatureKeyDerivationLength  
        {  
            get { return 128; }  
        }  
  
        public override int DefaultSymmetricKeyLength  
        {  
            get { return 128; }  
        }  
  
        public override string DefaultSymmetricKeyWrapAlgorithm  
        {  
            get { return SecurityAlgorithms.Aes128Encryption; }  
        }  
  
        public override string DefaultSymmetricSignatureAlgorithm  
        {  
            get { return SecurityAlgorithms.HmacSha1Signature; }  
        }  
  
        public override bool IsAsymmetricKeyLengthSupported(int length)  
        {  
            return length >= 1024 && length <= 4096;  
        }  
  
        public override bool IsSymmetricKeyLengthSupported(int length)  
        {  
            return length >= 128 && length <= 256;  
        }  
    }  
```  
  
## <a name="register-the-custom-algorithm"></a>注册自定义算法  

 注册可在配置文件或命令性代码中完成。 注册自定义算法时，要在实现加密服务提供程序的类与别名之间创建一个映射。 然后，该别名将映射到在 WCF 服务的绑定中指定算法时使用的 URI。 以下配置代码段揭示了如何在配置文件中注册自定义算法：  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
           <cryptoClasses>  
              <cryptoClass SHA256CSP="System.Security.Cryptography.SHA256CryptoServiceProvider, System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
           </cryptoClasses>  
           <nameEntry name="http://contoso.com/CustomAlgorithms/CustomHashAlgorithm"  
              class="SHA256CSP" />  
           </cryptoNameMapping>  
        </cryptographySettings>  
    </mscorlib>  
</configuration>  
```  
  
 <> 元素下的部分将 `cryptoClasses` 创建 SHA256CryptoServiceProvider 与别名 "SHA256CSP" 之间的映射。 <`nameEntry`> 元素将创建 "SHA256CSP" 别名与指定的 URL 之间的映射 `http://contoso.com/CustomAlgorithms/CustomHashAlgorithm` 。  
  
 要在代码中注册自定义算法，可使用 <xref:System.Security.Cryptography.CryptoConfig.AddAlgorithm(System.Type,System.String[])> 方法。 该方法可创建这两个映射。 以下示例揭示了如何调用此方法：  
  
```csharp
// Register the custom URI string defined for the hashAlgorithm in MyCustomAlgorithmSuite class to create the
// SHA256CryptoServiceProvider hash algorithm object.  
CryptoConfig.AddAlgorithm(typeof(SHA256CryptoServiceProvider), "http://contoso.com/CustomAlgorithms/CustomHashAlgorithm");  
```  
  
## <a name="configure-the-binding"></a>配置绑定  

 要配置绑定，可在绑定设置中指定自定义  <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 派生类，如下面的代码段所示：  
  
```csharp  
WSHttpBinding binding = new WSHttpBinding();  
            binding.Security.Message.AlgorithmSuite = new MyCustomAlgorithmSuite();  
```  
  
 有关完整的代码示例，请参阅 [WCF 安全中的加密灵活性](../samples/cryptographic-agility-in-wcf-security.md) 示例。  
  
## <a name="see-also"></a>另请参阅

- [保护服务和客户端的安全](../feature-details/securing-services-and-clients.md)
- [保证服务的安全](../securing-services.md)
- [安全性概述](../feature-details/security-overview.md)
- [安全性概念](../feature-details/security-concepts.md)
