---
title: 创建已签名和/或已加密的自定义标头
ms.date: 03/30/2017
ms.assetid: e8668b37-c79f-4714-9de5-afcb88b9ff02
ms.openlocfilehash: daa594950c25ea4a5c2012183c47231b4688719e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286716"
---
# <a name="creating-a-custom-header-that-is-signed-and-or-encrypted"></a>创建已签名和/或已加密的自定义标头

在使用 WCF 客户端调用非 WCF 服务时，有时需要使用自定义 SOAP 标头。 WCF 中有一个规范化 bug，该 bug 将阻止已签名和已加密的自定义标头使用非 WCF 服务。 此问题是因默认 XML 命名空间的规范化错误导致的。 此问题仅在使用已签名和/或已加密的自定义标头调用非 WCF 服务时发生。  当服务收到包含已签名和/或已加密的自定义标头的消息时，无法验证该签名。 此解决方法可避免出现规范化 bug，它允许与非 WCF 服务进行互操作，但不阻止与 WCF 服务进行互操作。  
  
## <a name="defining-the-custom-header"></a>定义自定义标头  

 自定义标头通过以下方式定义：定义消息协定并标记希望作为具有 <xref:System.ServiceModel.MessageHeaderAttribute> 属性的标头发送的成员。 若要修复规范化 bug，您必须确保 XML 序列化程序声明的是带前缀的自定义标头的命名空间而不是默认命名空间声明。 下面的代码演示如何定义将用作带正确的命名空间声明的消息标头的数据类型。  
  
```csharp
[System.CodeDom.Compiler.GeneratedCodeAttribute("svcutil", "3.0.4506.648")]  
[System.SerializableAttribute()]  
[System.Diagnostics.DebuggerStepThroughAttribute()]  
[System.ComponentModel.DesignerCategoryAttribute("code")]  
[System.Xml.Serialization.XmlTypeAttribute(AnonymousType=true, Namespace="http://www.example.org/getMessage/")]  
public partial class msgHeaderElement  
{  
   // Define the XML namespace and force it to use an ‘h’ prefix  
    [System.Xml.Serialization.XmlNamespaceDeclarations]  
    public System.Xml.Serialization.XmlSerializerNamespaces _xsns = new System.Xml.Serialization.XmlSerializerNamespaces(new System.Xml.XmlQualifiedName[] { new System.Xml.XmlQualifiedName("h", "http://www.example.org/getMessage/") });  
  
    private string msgHeaderInputField;  
  [System.Xml.Serialization.XmlElementAttribute(Form=System.Xml.Schema.XmlSchemaForm.Unqualified, Order=0)]  
    public string msgHeaderInput  
    {  
        get  
        {  
            return this.msgHeaderInputField;  
        }  
        set  
        {  
            this.msgHeaderInputField = value;  
        }  
    }  
}  
```  
  
 此代码声明一个将用 XML 序列化程序进行序列化的名为 `msgHeaderElement` 的新类型。 当序列化此类型的实例时，它将定义一个带前缀“h”的命名空间，从而修复规范化 bug。  然后，消息协定将定义 `msgHeaderElement` 的实例并用 <xref:System.ServiceModel.MessageHeaderAttribute> 属性标记它，如下面的示例所示。  
  
```csharp
[MessageContract]  
public  class MyMessageContract  
{  
   // other message contents...  
   [MessageHeader(ProductionLevel=ProtectionLevel.EncryptAndSign)]  
   public msgHeaderElement;  
   // other message contents...  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [默认消息协定](../samples/default-message-contract.md)
- [消息协定](../samples/message-contracts.md)
- [使用消息约定](using-message-contracts.md)
