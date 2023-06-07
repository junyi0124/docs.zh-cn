---
title: DataContractSerializer 示例
description: 此示例演示 WCF 中的 DataContractSerializer，它为数据协定类执行常规序列化和反序列化服务。
ms.date: 03/30/2017
helpviewer_keywords:
- XML Formatter
ms.assetid: e0a2fe89-3534-48c8-aa3c-819862224571
ms.openlocfilehash: d38dddcaff7316f4933207c4aa0897ad47306352
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253609"
---
# <a name="datacontractserializer-sample"></a>DataContractSerializer 示例

DataContractSerializer 示例演示为数据协定类执行常规序列化和反序列化服务的 <xref:System.Runtime.Serialization.DataContractSerializer>。 该示例创建一个 `Record` 对象，将其序列化到内存流，然后将该内存流反序列化回另一个 `Record` 对象，以演示的用法 <xref:System.Runtime.Serialization.DataContractSerializer> 。 然后示例使用二进制编写器序列化 `Record` 对象以演示编写器如何影响序列化。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 在下面的示例代码中显示 `Record` 的数据协定：  
  
```csharp  
[DataContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
internal class Record  
{  
    private double n1;  
    private double n2;  
    private string operation;  
    private double result;  
  
    internal Record(double n1, double n2, string operation, double result)  
    {  
        this.n1 = n1;  
        this.n2 = n2;  
        this.operation = operation;  
        this.result = result;  
    }  
  
    [DataMember]  
    internal double OperandNumberOne  
    {  
        get { return n1; }  
        set { n1 = value; }  
    }  
  
    [DataMember]  
    internal double OperandNumberTwo  
    {  
        get { return n2; }  
        set { n2 = value; }  
    }  
  
    [DataMember]  
    internal string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [DataMember]  
    internal double Result  
    {  
        get { return result; }  
        set { result = value; }  
    }  
  
    public override string ToString()  
    {  
        return $"Record: {n1} {operation} {n2} = {result}";
    }  
}  
```  
  
 该示例代码创建一个名为 `Record` 的 `record1` 对象，然后显示该对象。  
  
```csharp
Record record1 = new Record(1, 2, "+", 3);  
Console.WriteLine("Original record: {0}", record1.ToString());  
```  
  
 然后示例使用 <xref:System.Runtime.Serialization.DataContractSerializer> 将 `record1` 序列化为内存流。  
  
```csharp  
MemoryStream stream1 = new MemoryStream();  
  
//Serialize the Record object to a memory stream using DataContractSerializer.  
DataContractSerializer serializer = new DataContractSerializer(typeof(Record));  
serializer.WriteObject(stream1, record1);  
```  
  
 接下来，示例使用 <xref:System.Runtime.Serialization.DataContractSerializer> 将内存流反序列化回一个新的 `Record` 对象并显示它。  
  
```csharp  
stream1.Position = 0;  
  
//Deserialize the Record object back into a new record object.  
Record record2 = (Record)serializer.ReadObject(stream1);  
  
Console.WriteLine("Deserialized record: {0}", record2.ToString());  
```  
  
 在默认情况下，`DataContractSerializer` 使用 XML 的文本表示形式将对象编码为流。 但是，您可以通过传入不同的编写器来影响 XML 的编码。 本示例通过调用 <xref:System.Xml.XmlDictionaryWriter.CreateBinaryWriter%2A> 创建一个二进制编写器。 然后示例在调用 <xref:System.Runtime.Serialization.DataContractSerializer.WriteObjectContent%2A> 时将编写器和记录对象传递到序列化程序。 最后，示例刷新编写器并报告流的长度。  
  
```csharp  
MemoryStream stream2 = new MemoryStream();  
  
XmlDictionaryWriter binaryDictionaryWriter = XmlDictionaryWriter.CreateBinaryWriter(stream2);  
serializer.WriteObject(binaryDictionaryWriter, record1);  
binaryDictionaryWriter.Flush();  
  
//report the length of the streams  
Console.WriteLine("Text Stream is {0} bytes long", stream1.Length);  
Console.WriteLine("Binary Stream is {0} bytes long", stream2.Length);  
```  
  
 在运行该示例时，会显示原始记录和反序列化记录，接着是文本编码和二进制编码的长度之间的比较。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Original record: Record: 1 + 2 = 3  
Deserialized record: Record: 1 + 2 = 3  
Text Stream is 233 bytes long  
Binary Stream is 156 bytes long  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要启动该示例，请通过在命令提示符处键入 client\bin\client.exe 启动客户端。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\DataContractSerializer`  
