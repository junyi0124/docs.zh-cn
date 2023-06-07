---
title: 消息安全绑定
ms.date: 03/30/2017
ms.assetid: a4570ce7-864e-461b-85d8-0f7bcc53c2c8
ms.openlocfilehash: a4d13eabc0d086a9cfe58c95165b0405f60fcf14
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294373"
---
# <a name="message-security-binding"></a>消息安全绑定

本部分包含的示例演示了 WCF 中 Windows 服务中的消息安全绑定。  
  
## <a name="in-this-section"></a>本节内容  

 [匿名消息安全](message-security-anonymous.md)  
 此示例演示如何实现 Windows Communication Foundation (WCF) 应用程序，该应用程序使用不带客户端身份验证的消息级安全性，但需要使用服务器的 x.509 证书进行服务器身份验证。  
  
 [消息安全证书](message-security-certificate.md)  
 此示例演示如何实现一个应用程序，该应用程序对客户端使用 WS 安全性和 X.509 v3 证书身份验证，并要求使用服务器的 X.509 v3 证书进行服务器身份验证。  
  
 [用户名消息安全](message-security-user-name.md)  
 本示例演示如何实现一个应用程序，该应用程序对客户端使用具有用户名身份验证的 WS-Security，并要求使用服务器的 X.509v3 证书对服务器进行身份验证。  
  
 [Windows 消息安全](message-security-windows.md)  
 本示例演示如何将 <xref:System.ServiceModel.WSHttpBinding> 绑定配置为使用具有 Windows 身份验证的消息级安全性。
