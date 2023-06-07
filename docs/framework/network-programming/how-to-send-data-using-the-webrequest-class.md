---
title: 如何：使用 WebRequest 类发送数据
description: 了解如何使用 .NET Framework 中的 WebRequest 类将数据发送到服务器。 此过程通常用于将数据发布到网页。
ms.date: 03/25/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WebRequest class, sending data to a host
- Sending data to a host, using WebRequest class
ms.assetid: 66686878-38ac-4aa6-bf42-ffb568ffc459
ms.openlocfilehash: 4b353250fec778ee8b352f13de6d7faaf15c13ee
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502439"
---
# <a name="how-to-send-data-by-using-the-webrequest-class"></a>如何：使用 WebRequest 类发送数据

以下过程描述将数据发送到服务器的步骤。 此过程通常用于将数据发布到 Web 页。

## <a name="to-send-data-to-a-host-server"></a>将数据发送到主机服务器

1. 通过使用接受数据的资源（例如脚本或 ASP.NET 页面）的 URI 调用 <xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> 来创建 <xref:System.Net.WebRequest> 实例。 例如：

    ```csharp
    WebRequest request = WebRequest.Create("http://www.contoso.com/PostAccepter.aspx");
    ```

    ```vb
    Dim request as WebRequest = WebRequest.Create("http://www.contoso.com/PostAccepter.aspx")
    ```

    > [!NOTE]
    > .NET Framework 为以“http:”、“https:”、“ftp:”和“file:”开头的 URI 提供派生自 <xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse> 类的特定于协议的类   。

    如果需要设置或读取特定于协议的属性，必须将 <xref:System.Net.WebRequest> 或 <xref:System.Net.WebResponse> 转换为特定于协议的对象类型。 有关详细信息，请参阅[对可插入协议进行编程](programming-pluggable-protocols.md)。

2. 设置 `WebRequest` 对象中任何所需的属性值。 例如，若要启用身份验证，则将 <xref:System.Net.WebRequest.Credentials%2A?displayProperty=nameWithType> 属性设置为 <xref:System.Net.NetworkCredential> 类的实例：

    ```csharp
    request.Credentials = CredentialCache.DefaultCredentials;
    ```

    ```vb
    request.Credentials = CredentialCache.DefaultCredentials
    ```

3. 指定允许使用请求发送数据的协议方法，如 HTTP `POST` 方法：

    ```csharp
    request.Method = "POST";
    ```

    ```vb
    request.Method = "POST"
    ```

4. 将 <xref:System.Web.HttpRequest.ContentLength> 属性设置为请求中包含的字节数。 例如：

    ```csharp
    request.ContentLength = byteArray.Length;
    ```

    ```vb
    request.ContentLength = byteArray.Length
    ```

5. 将 <xref:System.Web.HttpRequest.ContentType> 属性设置为适当的值。 例如：

    ```csharp
    request.ContentType = "application/x-www-form-urlencoded";
    ```

    ```vb
    request.ContentType = "application/x-www-form-urlencoded"
    ```

6. 通过调用 <xref:System.Net.WebRequest.GetRequestStream%2A> 方法获取包含请求数据的流。 例如：

    ```csharp
    Stream dataStream = request.GetRequestStream();
    ```

    ```vb
    Dim dataStream As Stream = request.GetRequestStream()
    ```

7. 将数据写入到 `GetRequestStream` 方法返回的 <xref:System.IO.Stream> 对象中。 例如：

    ```csharp
    dataStream.Write(byteArray, 0, byteArray.Length);
    ```

    ```vb
    dataStream.Write(byteArray, 0, byteArray.Length)
    ```

8. 通过调用 <xref:System.IO.Stream.Close%2A?displayProperty=nameWithType> 方法关闭请求流。 例如：

    ```csharp
    dataStream.Close();
    ```

    ```vb
    dataStream.Close()
    ```

9. 通过调用 <xref:System.Net.WebRequest.GetResponse%2A?displayProperty=nameWithType> 向服务器发送请求。 此方法返回包含服务器响应的对象。 返回的 `WebResponse` 对象的类型由请求的 URI 的方案决定。 例如：

    ```csharp
    WebResponse response = request.GetResponse();
    ```

    ```vb
    Dim response As WebResponse = request.GetResponse()
    ```

10. 可以访问 `WebResponse` 对象的属性或将其强制转换为特定于协议的实例，以读取特定于协议的属性。

    例如，要访问 <xref:System.Net.HttpWebResponse> 特定于 HTTP 的属性，请将 `WebResponse` 对象强制转换为 <xref:System.Net.HttpWebResponse> 引用。 以下代码示例演示如何显示与响应一起发送的特定于 HTTP 的 <xref:System.Net.HttpWebResponse.StatusDescription%2A?displayProperty=nameWithType> 属性：

    ```csharp
    Console.WriteLine(((HttpWebResponse)response).StatusDescription);
    ```

    ```vb
    Console.WriteLine(CType(response, HttpWebResponse).StatusDescription)
    ```

11. 若要获取包含由服务器发送的响应数据的流，请调用 `WebResponse` 的 <xref:System.Net.WebResponse.GetResponseStream%2A?displayProperty=nameWithType> 方法。 例如：

    ```csharp
    Stream dataStream = response.GetResponseStream();
    ```

    ```vb
    Dim dataStream As Stream = response.GetResponseStream()
    ```

12. 从响应对象读取数据后，可以使用 <xref:System.Net.WebResponse.Close%2A?displayProperty=nameWithType> 方法关闭它，也可以使用 <xref:System.IO.Stream.Close%2A?displayProperty=nameWithType> 方法关闭响应流。 如果不关闭响应或流，则应用程序可能会耗尽服务器连接，并且无法处理其他请求。 因为 `WebResponse.Close` 方法在其关闭响应时调用 `Stream.Close`，所以不必在响应和流对象上调用 `Close`，尽管这样做没有坏处。 例如：

    ```csharp
    response.Close();
    ```

    ```vb
    response.Close()
    ```

## <a name="example"></a>示例

以下示例演示如何将数据发送到 Web 服务器并读取其响应中的数据：

[!code-csharp[SendDataUsingWebRequest](../../../samples/snippets/csharp/VS_Snippets_Network/SendDataUsingWebRequest/cs/WebRequestPostExample.cs)]
[!code-vb[SendDataUsingWebRequest](../../../samples/snippets/visualbasic/VS_Snippets_Network/SendDataUsingWebRequest/vb/WebRequestPostExample.vb)]

## <a name="see-also"></a>请参阅

- [创建 Internet 请求](creating-internet-requests.md)
- [在网络上使用流](using-streams-on-the-network.md)
- [通过代理访问 Internet](accessing-the-internet-through-a-proxy.md)
- [请求数据](requesting-data.md)
- [如何：使用 WebRequest 类请求数据](how-to-request-data-using-the-webrequest-class.md)
