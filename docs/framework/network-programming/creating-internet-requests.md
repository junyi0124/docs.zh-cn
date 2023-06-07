---
title: 创建 Internet 请求
description: 了解应用程序如何通过 WebRequest.Create 方法创建 WebRequest 实例，该方法可基于传递给它的 URI 方案创建派生类。
ms.date: 03/30/2017
helpviewer_keywords:
- WebRequest class, sending and receiving data
- Networking
- HttpWebResponse class, sending and receiving data
- requesting data from Internet, creating requests
- Network Resources
- Internet, requesting data
- data requests, creating requests
ms.assetid: faab683e-3f1e-4eee-b5e9-59f7245033d5
ms.openlocfilehash: 389c7bf5969eca4322aa680a6f28dec0b899709a
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325642"
---
# <a name="create-internet-requests"></a>创建 Internet 请求

应用程序通过 <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> 方法创建 <xref:System.Net.WebRequest> 实例。 <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> 是一种基于传递给它的 URI 方案创建从 <xref:System.Net.WebRequest> 派生的类的静态方法。  
  
## <a name="web-file-and-ftp-requests"></a>Web、文件和 FTP 请求

.NET Framework 提供从 `WebRequest` 派生的 <xref:System.Net.HttpWebRequest> 类，用以处理 HTTP 和 HTTPS 请求。 在大多数情况下，`WebRequest` 类提供提出请求所需的所有属性；但是，如有必要，将 `WebRequest.Create` 方法创建的 `WebRequest` 对象转换为 `HttpWebRequest` 类型，即可访问该请求特定于 HTTP 的属性。 同样，`HttpWebResponse` 对象可处理来自 HTTP 和 HTTPS 请求的响应。 若要访问 `HttpWebResponse` 对象的特定于 HTTP 的属性，需要将 `WebResponse` 对象转换为 `HttpWebResponse` 类型。  
  
.NET Framework 还提供 <xref:System.Net.FileWebRequest> 和 <xref:System.Net.FileWebResponse> 类来处理使用“file:”URI 方案的资源请求。 同样，提供的 <xref:System.Net.FtpWebRequest> 和 <xref:System.Net.FtpWebResponse> 类可处理使用“ftp:”方案的资源请求。 如果请求的是使用上述任一方案的资源，则可使用 `WebRequest.Create` 方法获取用于发出请求的对象。  
  
要处理使用其他应用程序级协议的请求，请实现从 `WebRequest` 和 `WebResponse` 派生的协议特定的类。 有关详细信息，请参阅[对可插入协议进行编程](programming-pluggable-protocols.md)。  
  
## <a name="see-also"></a>请参阅

- [如何：使用 WebRequest 类请求数据](how-to-request-data-using-the-webrequest-class.md)
- [请求数据](requesting-data.md)
