---
title: 将算法名称映射到加密类
description: 在 .NET 中将算法名称映射到加密类。 开发人员有四个选项用于创建加密对象。
ms.date: 03/30/2017
helpviewer_keywords:
- mapping algorithm names
- cryptography, mapping algorithm names
- cryptographic algorithms
- names [.NET Framework], algorithm mapping
ms.assetid: 01327c69-c5e1-4ef6-b73f-0a58351f0492
ms.openlocfilehash: b67db612496e56a341dab2e5fc4b52c954ff02b4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183385"
---
# <a name="mapping-algorithm-names-to-cryptography-classes"></a>将算法名称映射到加密类

开发人员可通过以下四种方式使用 Windows SDK 创建加密对象：  
  
- 使用 **new** 运算符创建对象。  
  
- 通过对该算法的抽象类调用 **create** 方法，创建实现特定加密算法的对象。  
  
- 通过调用方法，创建实现特定加密算法的对象 <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A?displayProperty=nameWithType> 。  
  
- 创建一个对象，该对象实现一类加密算法 (如对称块加密) 方法是在该类型算法的抽象类上调用 **Create** 方法 (例如 <xref:System.Security.Cryptography.SymmetricAlgorithm>) 。  
  
 例如，假设开发人员想要计算一组字节的 SHA1 哈希。 <xref:System.Security.Cryptography>命名空间包含 SHA1 算法的两个实现，一个纯粹的托管实现，另一个包装 CryptoAPI。 开发人员可以选择实例化特定的 SHA1 实现 (如 <xref:System.Security.Cryptography.SHA1Managed>) 通过调用 **new** 运算符来实现）。 但是，如果公共语言运行时加载哪个类（只要类实现 SHA1 哈希算法），开发人员就可以通过调用方法来创建对象 <xref:System.Security.Cryptography.SHA1.Create%2A?displayProperty=nameWithType> 。 此方法调用 **CryptoConfig. cryptoconfig.createfromname ( "" **，该 ) 必须返回 SHA1 哈希算法的实现。）。  
  
 开发人员还可以调用 **CryptoConfig. cryptoconfig.createfromname ( "SHA1" ) ** ，因为在默认情况下，加密配置包括 .NET Framework 附带的算法的短名称。  
  
 如果使用的哈希算法并不重要，开发人员可以调用 <xref:System.Security.Cryptography.HashAlgorithm.Create%2A?displayProperty=nameWithType> 方法，这将返回实现哈希转换的对象。  
  
## <a name="mapping-algorithm-names-in-configuration-files"></a>在配置文件中映射算法名称  

 默认情况下，运行时 <xref:System.Security.Cryptography.SHA1CryptoServiceProvider> 为所有四个方案返回一个对象。 但是，计算机管理员可以更改最后两个方案中的方法返回的对象的类型。 为此，必须将友好算法名称映射到要在计算机配置文件中使用的类 ( # A0) 。  
  
 下面的示例演示如何配置运行时，使**CryptoConfig，cryptoconfig.createfromname ( "SHA1" ) **，以及**HashAlgorithm**返回对象**的类型为**""，然后返回一个 `MySHA1HashClass` 对象。  
  
```xml  
<configuration>  
   <!-- Other configuration settings. -->  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass MySHA1Hash="MySHA1HashClass, MyAssembly  
                  Culture='en', PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="SHA1" class="MySHA1Hash"/>  
            <nameEntry name="System.Security.Cryptography.SHA1"  
                       class="MySHA1Hash"/>  
            <nameEntry name="System.Security.Cryptography.HashAlgorithm"  
                       class="MySHA1Hash"/>  
         </cryptoNameMapping>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
 您可以在 [<的 cryptoClass \> 元素](./file-schema/cryptography/cryptoclass-element.md) 中指定属性的名称 (上一个示例将该属性命名 `MySHA1Hash`) 。 元素中的属性的值 **\<cryptoClass>** 是公共语言运行时用来查找类的字符串。 您可以使用满足指定 [完全限定的类型名称](../reflection-and-codedom/specifying-fully-qualified-type-names.md)中指定的要求的任何字符串。  
  
 许多算法名称可以映射到同一个类。 [ \<nameEntry> 元素](./file-schema/cryptography/nameentry-element.md)将类映射到一个友好算法名称。 **Name**属性可以是在调用**CryptoConfig. cryptoconfig.createfromname**方法时使用的字符串，也可以是命名空间中抽象加密类的名称 <xref:System.Security.Cryptography> 。 **Class**特性的值是元素中的特性的名称 **\<cryptoClass>** 。  
  
> [!NOTE]
> 可以通过调用 <xref:System.Security.Cryptography.SHA1.Create%2A?displayProperty=nameWithType> 或 **CryptoConfig. cryptoconfig.createfromname ( "sha1" ) ** 方法来获取 SHA1 算法。 每个方法仅保证返回实现 SHA1 算法的对象。 不需要将算法的每个友好名称映射到配置文件中的相同类。  
  
 有关默认名称及其映射到的类的列表，请参阅 <xref:System.Security.Cryptography.CryptoConfig> 。  
  
## <a name="see-also"></a>请参阅

- [加密服务](../../standard/security/cryptographic-services.md)
- [配置加密类](configure-cryptography-classes.md)
