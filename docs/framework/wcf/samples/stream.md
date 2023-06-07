---
title: Stream
ms.date: 03/30/2017
ms.assetid: 58a3db81-20ab-4627-bf31-39d30b70b4fe
ms.openlocfilehash: 04bc5db4172d9052b45f63a2f7ed0fb997dc4c6c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257192"
---
# <a name="stream"></a>Stream

流示例演示流传输模式通信的用法。 收发流的若干操作都由服务公开。 此示例是自承载的。 客户端和服务都是控制台程序。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 Windows Communication Foundation (WCF) 可以在两种传输模式（缓冲或流式处理）中进行通信。 在默认的缓冲传输模式中，必须完整传递消息，接收方才能读取该消息。 在流传输模式下，接收方可以在完整传递消息之前开始处理该消息。 当传递的信息很长且可以依次处理时，流模式非常有用。 当消息过长以致于无法全部缓冲时，流模式也非常有用。  
  
## <a name="streaming-and-service-contracts"></a>流处理和服务协定  

 在设计服务协定时，可以考虑进行流处理。 如果某个操作接收或返回大量数据，则应当考虑对该数据进行流处理，以免因对输入或输出消息进行缓冲而导致内存使用率过高。 若要对数据进行流处理，用来存放该数据的参数必须是消息中的唯一参数。 例如，如果要对输入消息进行流处理，则该操作必须正好具有一个输入参数。 同样，如果要对输出消息进行流处理，则该操作必须正好具有一个输出参数或一个返回值。 在任一情况下，该参数或返回值类型必须是 `Stream`、`Message` 或 `IXmlSerializable`。 下面是该流处理示例中使用的服务协定。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IStreamingSample  
{  
    [OperationContract]  
    Stream GetStream(string data);  
    [OperationContract]  
    bool UploadStream(Stream stream);  
    [OperationContract]  
    Stream EchoStream(Stream stream);  
    [OperationContract]  
    Stream GetReversedStream();  
  
}  
```  
  
 `GetStream` 操作接收一些输入数据作为经过缓冲的字符串，并返回经过流处理的 `Stream`。 相反，`UploadStream` 接收一个经过流处理的 `Stream`，并返回一个经过缓冲的 `bool`。 `EchoStream` 是输入消息和输出消息都经过流处理的操作的示例，它接收和返回 `Stream`。 最后，`GetReversedStream` 将不接收输入，并返回一个 `Stream`（已经过流处理）。  
  
## <a name="enabling-streamed-transfers"></a>启用流传输  

 按照上面的说明定义操作协定将在编程模型级别提供流处理。 如果仅限于此，则传输仍将缓冲整个消息内容。 若要启用传输流，请在传输的绑定元素上选择传输模式。 该绑定元素具有一个 `TransferMode` 属性，该属性可以设置为 `Buffered`、`Streamed`、`StreamedRequest` 或 `StreamedResponse`。 将传输模式设置为 `Streamed` 将在两个方向上启用流通信。 将传输模式设置为 `StreamedRequest` 或 `StreamedResponse` 将分别只在请求或响应中启用流通信。  
  
 `basicHttpBinding` 公开绑定上的 `TransferMode` 属性，这与 `NetTcpBinding` 和 `NetNamedPipeBinding` 一样。 对于其他传输，必须创建一个自定义绑定以设置传输模式。  
  
 示例中的如下配置代码演示如何将 `TransferMode` 和自定义 HTTP 绑定上的 `basicHttpBinding` 属性设置为流处理：  
  
```xml  
<!-- An example basicHttpBinding using streaming. -->  
<basicHttpBinding>  
  <binding name="HttpStreaming" maxReceivedMessageSize="67108864"  
           transferMode="Streamed"/>  
</basicHttpBinding>  
<!-- An example customBinding using HTTP and streaming.-->  
<customBinding>  
  <binding name="Soap12">  
    <textMessageEncoding messageVersion="Soap12WSAddressing10" />  
    <httpTransport transferMode="Streamed"  
                   maxReceivedMessageSize="67108864"/>  
  </binding>  
</customBinding>  
```  
  
 除了将 `transferMode` 设置为 `Streamed` 以外，上面的配置代码还将 `maxReceivedMessageSize` 设置为 64 MB。 作为一个防范机制，`maxReceivedMessageSize` 为允许接收的最大消息设置了上限。 默认的 `maxReceivedMessageSize` 为 64 KB，对于流处理方案而言，此值通常太低。  
  
## <a name="processing-data-as-it-is-streamed"></a>处理经过流处理的数据  

 `GetStream`、`UploadStream` 和 `EchoStream` 操作都可以处理直接从文件发送数据或将接收的数据直接保存到文件的情况。 但是，在某些情况下，需要发送或接收大量数据，并对正在发送或接收的数据块执行某种处理。 解决类似方案的一种方法是编写一个自定义流（一个派生自 <xref:System.IO.Stream> 的类）来处理读取或写入的数据。 `GetReversedStream` 操作和 `ReverseStream` 类就是类似方法的一个示例。  
  
 `GetReversedStream` 创建并返回 `ReverseStream` 的新实例。 当系统从该 `ReverseStream` 对象中进行读取时，会发生实际的处理。 `ReverseStream.Read` 实现从基础文件中读取字节块区，反转字节，然后返回经过反转的字节。 这不会反转整个文件内容，而是一次仅反转一个字节块区。 下面的示例演示在流中读取或写入内容时如何进行流处理。  
  
```csharp
class ReverseStream : Stream  
{  
  
    FileStream inStream;  
    internal ReverseStream(string filePath)  
    {  
        //Opens the file and places a StreamReader around it.  
        inStream = File.OpenRead(filePath);  
    }  
  
    // Other methods removed for brevity.  
  
    public override int Read(byte[] buffer, int offset, int count)  
    {  
        int countRead=inStream.Read(buffer, offset,count);  
        ReverseBuffer(buffer, offset, countRead);  
        return countRead;  
    }  
  
    public override void Close()  
    {  
        inStream.Close();  
        base.Close();  
    }  
    protected override void Dispose(bool disposing)  
    {  
        inStream.Dispose();  
        base.Dispose(disposing);  
    }  
    void ReverseBuffer(byte[] buffer, int offset, int count)  
    {  
        int i, j;  
        for (i = offset, j = offset + count - 1; i < j; i++, j--)  
        {  
            byte currenti = buffer[i];  
            buffer[i] = buffer[j];  
            buffer[j] = currenti;  
        }  
  
    }  
}  
```  
  
## <a name="running-the-sample"></a>运行示例  

 若要运行此示例，请首先按照本文档末尾的说明生成服务和客户端， 然后在两个不同的控制台窗口中启动服务和客户端。 当客户端启动时，它会在服务就绪时等待您按 Enter。 客户端随后会依次通过 HTTP 和 TCP 来调用 `GetStream()`、`UploadStream()` 和 `GetReversedStream()` 方法。 下面是服务的示例输出，其后面是客户端的示例输出：  
  
 服务输出：  
  
```console  
The streaming service is ready.  
Press <ENTER> to terminate service.  
  
Saving to file D:\...\uploadedfile  
......................  
File D:\...\uploadedfile saved  
Saving to file D:\...\uploadedfile  
...............  
File D:\...\uploadedfile saved  
```  
  
 客户端输出：  
  
```console  
Press <ENTER> when service is ready  
------ Using HTTP ------
Calling GetStream()  
Saving to file D:\...\clientfile  
......................  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
Calling UploadStream()  
Calling GetReversedStream()  
Saving to file D:\...\clientfile  
......................  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
------ Using Custom HTTP ------  
Calling GetStream()  
Saving to file D:\...\clientfile  
...............  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
Calling UploadStream()  
Calling GetReversedStream()  
Saving to file D:\...\clientfile  
...............  
Wrote 33405 bytes to stream  
  
File D:\...\clientfile saved  
  
Press <ENTER> to terminate client.  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!NOTE]
> 如果使用 Svcutil.exe 为此示例重新生成配置，请确保在客户端配置中修改终结点名称以与客户端代码匹配。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Stream`  
