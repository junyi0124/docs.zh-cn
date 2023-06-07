---
title: 对可插入协议进行编程
description: 了解抽象的 WebRequest 和 WebResponse 类如何支持可插入协议，该协议允许应用程序在不指定协议的情况下获取数据。
ms.date: 03/30/2017
helpviewer_keywords:
- downloading Internet resources, pluggable protocols
- WebRequest class, pluggable protocols
- response to Internet request, pluggable protocols
- WebResponse class, pluggable protocols
- sending data, pluggable protocols
- network resources, pluggable protocols
- Internet, pluggable protocols
- programming pluggable protocols
- pluggable protocols, programming
- requesting data from Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
ms.assetid: 66ef8456-7576-4e97-8956-959b216373db
ms.openlocfilehash: a3f8106b238c28de77362e73aa26667209f6b517
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263173"
---
# <a name="programming-pluggable-protocols"></a>对可插入协议进行编程

<xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse> 抽象类为可插入协议提供了基础。 通过从 <xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse> 派生协议特定的类，应用程序可以请求 Internet 资源中的数据并读取响应而无需指定所使用的协议。  
  
 在创建协议特定的 <xref:System.Net.WebRequest> 前，必须首先注册其 Create 方法。 使用<xref:System.Net.WebRequest> 的静态 <xref:System.Net.WebRequest.RegisterPrefix%28System.String%2CSystem.Net.IWebRequestCreate%29> 方法注册一个 <xref:System.Net.WebRequest> 子代来处理一组请求（包括对某一特定 Internet 方案的请求、对某个方案和服务器的请求或对某个方案、服务器和路径的请求）。  
  
 在大多数情况下，可以使用 <xref:System.Net.WebRequest> 类的方法和属性发送和接收数据。 但是，如果需要访问协议特定的属性，则可以将 <xref:System.Net.WebRequest> 的类型转换成特定的派生类实例。  
  
 要利用可插入协议，<xref:System.Net.WebRequest> 子代必须提供一个默认的“请求并响应”事务，该事务不要求设置协议特定的属性。 例如 <xref:System.Net.HttpWebRequest> 类实现了 HTTP 的 <xref:System.Net.WebRequest> 类，默认情况下提供 `GET` 请求并返回包含从 Web 服务器返回的流的 <xref:System.Net.HttpWebResponse>。  
  
## <a name="see-also"></a>请参阅

- [从 WebRequest 派生](deriving-from-webrequest.md)
- [从 WebResponse 派生](deriving-from-webresponse.md)
- [.NET Framework 中的网络编程](index.md)
- [如何：转换 WebRequest 以访问协议特定属性](how-to-typecast-a-webrequest-to-access-protocol-specific-properties.md)
