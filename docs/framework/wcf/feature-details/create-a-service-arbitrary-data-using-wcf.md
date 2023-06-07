---
title: 如何：使用 WCF REST 编程模型创建接受任意数据的服务
ms.date: 03/30/2017
ms.assetid: e566c15a-b600-4e4a-be3a-4af43e767dae
ms.openlocfilehash: 9c9899705861cc1cf2cda2559c30a0a60d8cc635
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96237828"
---
# <a name="how-to-create-a-service-that-accepts-arbitrary-data-using-the-wcf-rest-programming-model"></a>如何：使用 WCF REST 编程模型创建接受任意数据的服务

有时，开发人员必须完全控制从服务操作返回数据的方式。 如果服务操作必须以不支持的格式返回数据 byWCF，则会出现这种情况。 本主题讨论如何使用 WCF REST 编程模型创建接收任意数据的服务。  
  
### <a name="to-implement-the-service-contract"></a>实现服务协定  
  
1. 定义服务协定。 接收任意数据的操作必须具有一个类型为 <xref:System.IO.Stream> 的参数。 此外，此参数必须为传入请求正文的唯一参数。 本示例中介绍的操作还采用一个文件名参数。 此参数在请求的 URL 中传递。 通过在 <xref:System.UriTemplate> 中指定 <xref:System.ServiceModel.Web.WebInvokeAttribute> 可以指定在 URL 中传递参数。 在这种情况下，用于调用此方法的 URI 以 "UploadFile/Filename" 结尾。 URI 模板的 "{filename}" 部分指定在用于调用操作的 URI 内传递操作的 filename 参数。  
  
    ```csharp  
     [ServiceContract]  
    public interface IReceiveData  
    {  
        [WebInvoke(UriTemplate = "UploadFile/{fileName}")]  
        void UploadFile(string fileName, Stream fileContents);  
    }  
    ```  
  
2. 实现服务协定。 该协定只有一个方法 `UploadFile`，该方法以流的形式接收包含任意数据的文件。 操作读取该数据流，同时对所读字节数进行计数，然后显示文件名和所读字节数。  
  
    ```csharp  
    public class RawDataService : IReceiveData  
    {  
        public void UploadFile(string fileName, Stream fileContents)  
        {  
            byte[] buffer = new byte[10000];  
            int bytesRead, totalBytesRead = 0;  
            do  
            {  
                bytesRead = fileContents.Read(buffer, 0, buffer.Length);  
                totalBytesRead += bytesRead;  
            } while (bytesRead > 0);  
            Console.WriteLine("Service: Received file {0} with {1} bytes", fileName, totalBytesRead);  
        }  
    }  
    ```  
  
### <a name="to-host-the-service"></a>承载服务  
  
1. 创建用于承载服务的控制台应用程序。  
  
    ```csharp  
    class Program  
    {  
       static void Main(string[] args)  
       {  
       }  
    }  
    ```  
  
2. 在 `Main` 方法中创建一个变量以保存服务的基址。  
  
    ```csharp  
    string baseAddress = "http://" + Environment.MachineName + ":8000/Service";  
    ```  
  
3. 创建服务的一个 <xref:System.ServiceModel.ServiceHost> 实例，该实例指定服务类和基址。  
  
    ```csharp  
    ServiceHost host = new ServiceHost(typeof(RawDataService), new Uri(baseAddress));  
    ```  
  
4. 添加一个指定协定、<xref:System.ServiceModel.WebHttpBinding> 和 <xref:System.ServiceModel.Description.WebHttpBehavior> 的终结点。  
  
    ```csharp  
    host.AddServiceEndpoint(typeof(IReceiveData), new WebHttpBinding(), "").Behaviors.Add(new WebHttpBehavior());  
    ```  
  
5. 打开服务主机。 现在服务已准备好接收请求。  
  
    ```csharp  
    host.Open();  
    Console.WriteLine("Host opened");  
    ```  
  
### <a name="to-call-the-service-programmatically"></a>以编程方式调用服务  
  
1. 使用用于调用服务的 URI 创建一个 <xref:System.Net.HttpWebRequest>。 在此代码中，基址与 `"/UploadFile/Text"` 组合在一起。 URI 的 `"UploadFile"` 部分指定要调用的操作。 URI 的 `"Test.txt"` 部分指定要传递给 `UploadFile` 操作的文件名参数。 这两项都映射到应用于操作协定的 <xref:System.UriTemplate>。  
  
    ```csharp  
    HttpWebRequest req = (HttpWebRequest)HttpWebRequest.Create(baseAddress + "/UploadFile/Test.txt");  
    ```  
  
2. 将 <xref:System.Net.HttpWebRequest.Method%2A> 的 <xref:System.Net.HttpWebRequest> 属性设置为 `POST`，并将 <xref:System.Net.HttpWebRequest.ContentType%2A> 属性设置为 `"text/plain"`。 这会告知服务代码正在发送数据，且数据的格式为纯文本。  
  
    ```csharp  
    req.Method = "POST";  
    req.ContentType = "text/plain";  
    ```  
  
3. 调用 <xref:System.Net.HttpWebRequest.GetRequestStream%2A> 以获取请求流，创建要发送的数据，将该数据写入请求流，然后关闭请求流。  
  
    ```csharp  
    Stream reqStream = req.GetRequestStream();  
    byte[] fileToSend = new byte[12345];  
    for (int i = 0; i < fileToSend.Length; i++)  
       {  
           fileToSend[i] = (byte)('a' + (i % 26));  
       }  
    reqStream.Write(fileToSend, 0, fileToSend.Length);  
    reqStream.Close();  
    ```  
  
4. 通过调用 <xref:System.Net.HttpWebRequest.GetResponse%2A> 从服务获取响应，然后向控制台显示响应数据。  
  
    ```csharp  
    HttpWebResponse resp = (HttpWebResponse)req.GetResponse();  
    Console.WriteLine("Client: Receive Response HTTP/{0} {1} {2}", resp.ProtocolVersion, (int)resp.StatusCode, resp.StatusDescription);  
    ```  
  
5. 关闭服务主机。  
  
    ```csharp  
    host.Close();  
    ```  
  
## <a name="example"></a>示例  

 下面列出了此示例的完整代码。  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Text;  
using System.ServiceModel;  
using System.ServiceModel.Web;  
using System.ServiceModel.Description;  
using System.IO;  
using System.Net;  
  
namespace ReceiveRawData  
{  
    [ServiceContract]  
    public interface IReceiveData  
    {  
        [WebInvoke(UriTemplate = "UploadFile/{fileName}")]  
        void UploadFile(string fileName, Stream fileContents);  
    }  
    public class RawDataService : IReceiveData  
    {  
        public void UploadFile(string fileName, Stream fileContents)  
        {  
            byte[] buffer = new byte[10000];  
            int bytesRead, totalBytesRead = 0;  
            do  
            {  
                bytesRead = fileContents.Read(buffer, 0, buffer.Length);  
                totalBytesRead += bytesRead;  
            } while (bytesRead > 0);  
            Console.WriteLine("Service: Received file {0} with {1} bytes", fileName, totalBytesRead);  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            string baseAddress = "http://" + Environment.MachineName + ":8000/Service";  
            ServiceHost host = new ServiceHost(typeof(RawDataService), new Uri(baseAddress));  
            host.AddServiceEndpoint(typeof(IReceiveData), new WebHttpBinding(), "").Behaviors.Add(new WebHttpBehavior());  
            host.Open();  
            Console.WriteLine("Host opened");  
  
            HttpWebRequest req = (HttpWebRequest)HttpWebRequest.Create(baseAddress + "/UploadFile/Test.txt");  
            req.Method = "POST";  
            req.ContentType = "text/plain";  
            Stream reqStream = req.GetRequestStream();  
            byte[] fileToSend = new byte[12345];  
            for (int i = 0; i < fileToSend.Length; i++)  
            {  
                fileToSend[i] = (byte)('a' + (i % 26));  
            }  
            reqStream.Write(fileToSend, 0, fileToSend.Length);  
            reqStream.Close();  
            HttpWebResponse resp = (HttpWebResponse)req.GetResponse();  
            Console.WriteLine("Client: Receive Response HTTP/{0} {1} {2}", resp.ProtocolVersion, (int)resp.StatusCode, resp.StatusDescription);  
            host.Close();  
  
        }  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 编译该代码时，请引用 System.ServiceModel.dll 和 System.ServiceModel.Web.dll  
  
## <a name="see-also"></a>另请参阅

- [UriTemplate 和 UriTemplateTable](uritemplate-and-uritemplatetable.md)
- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
- [WCF Web HTTP 编程模型概述](wcf-web-http-programming-model-overview.md)
