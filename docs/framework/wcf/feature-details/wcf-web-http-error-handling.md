---
title: WCF Web HTTP 错误处理
ms.date: 03/30/2017
ms.assetid: 02891563-0fce-4c32-84dc-d794b1a5c040
ms.openlocfilehash: cfbf98c69370764a9526c32459d43521177476e3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239024"
---
# <a name="wcf-web-http-error-handling"></a>WCF Web HTTP 错误处理

Windows Communication Foundation (WCF) Web HTTP 错误处理使你能够从 WCF Web HTTP 服务返回错误，该服务指定 HTTP 状态代码，并使用与操作 (的格式相同的格式（例如，XML 或 JSON) ）返回错误详细信息。  
  
## <a name="wcf-web-http-error-handling"></a>WCF Web HTTP 错误处理  

 <xref:System.ServiceModel.Web.WebFaultException> 类定义可用于指定 HTTP 状态代码的构造函数。 随后会将此状态代码返回到客户端。 <xref:System.ServiceModel.Web.WebFaultException> 类的泛型版本 <xref:System.ServiceModel.Web.WebFaultException%601> 可用于返回用户定义的类型，该类型中包含有关所出现错误的信息。 将使用由操作指定的格式序列化此自定义对象，并将它返回到客户端。 下面的示例演示如何返回 HTTP 状态代码。  
  
```csharp
public string Operation1()
{
    // Operation logic  
   // ...
   throw new WebFaultException(HttpStatusCode.Forbidden);
}  
```  
  
 下面的示例演示如何在用户定义的类型中返回 HTTP 状态代码和额外信息。 `MyErrorDetail` 是用户定义的类型，其中包含有关所出现错误的额外信息。  
  
```csharp
public string Operation2()
{
   // Operation logic  
   // ...
   MyErrorDetail detail = new MyErrorDetail()
   {  
      Message = "Error Message",  
      ErrorCode = 123,  
   }  
   throw new WebFaultException<MyErrorDetail>(detail, HttpStatusCode.Forbidden);  
}  
```  
  
 上述代码返回 HTTP 响应，其中包括禁止状态代码以及包含 `MyErrorDetails` 对象实例的正文。 `MyErrorDetails` 对象的格式由以下内容确定：  
  
- 服务操作指定的 `ResponseFormat` 或 <xref:System.ServiceModel.Web.WebGetAttribute> 特性的 <xref:System.ServiceModel.Web.WebInvokeAttribute> 参数值。  
  
- <xref:System.ServiceModel.Description.WebHttpBehavior.AutomaticFormatSelectionEnabled%2A> 的值。  
  
- 通过访问 <xref:System.ServiceModel.Web.OutgoingWebResponseContext.Format%2A> 得到的 <xref:System.ServiceModel.Web.OutgoingWebResponseContext> 属性值。  
  
 有关这些值如何影响操作格式的详细信息，请参阅 [WCF WEB HTTP 格式设置](wcf-web-http-formatting.md)。  
  
 <xref:System.ServiceModel.Web.WebFaultException> 是一个 <xref:System.ServiceModel.FaultException>，因此可用作公开 SOAP 终结点和 Web HTTP 终结点的服务的错误异常编程模型。  
  
## <a name="see-also"></a>另请参阅

- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
- [WCF Web HTTP 格式设置](wcf-web-http-formatting.md)
- [定义和指定错误](../defining-and-specifying-faults.md)
- [处理异常和错误](../extending/handling-exceptions-and-faults.md)
- [发送和接收错误](../sending-and-receiving-faults.md)
