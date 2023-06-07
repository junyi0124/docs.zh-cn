---
title: XMLSerializer 示例
ms.date: 03/30/2017
ms.assetid: 7d134453-9a35-4202-ba77-9ca3a65babc3
ms.openlocfilehash: 47fdcfeda796d99740a49997ee85fc63fbb27b21
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96237568"
---
# <a name="xmlserializer-sample"></a>XMLSerializer 示例

此示例演示如何序列化和反序列化与 <xref:System.Xml.Serialization.XmlSerializer> 兼容的类型。  (WCF) 格式化程序的默认 Windows Communication Foundation 为 <xref:System.Runtime.Serialization.DataContractSerializer> 类。 当无法使用 <xref:System.Xml.Serialization.XmlSerializer> 类时，可以使用 <xref:System.Runtime.Serialization.DataContractSerializer> 类来序列化和反序列化类型。 当需要精确控制 XML 时通常会发生这种情况 - 例如，如果某个数据必须是一个 XML 属性，而不能是 XML 元素。 此外， <xref:System.Xml.Serialization.XmlSerializer> 在为非 WCF 服务创建客户端时，经常会自动选择。  
  
 在此示例中，客户端是一个控制台应用程序 (.exe)，服务是由 Internet 信息服务 (IIS) 承载的。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 必须应用于该接口，如下面的示例代码所示。  
  
```csharp  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"), XmlSerializerFormat]  
public interface IXmlSerializerCalculator  
{  
    [OperationContract]  
    ComplexNumber Add(ComplexNumber n1, ComplexNumber n2);  
    [OperationContract]  
    ComplexNumber Subtract(ComplexNumber n1, ComplexNumber n2);  
    [OperationContract]  
    ComplexNumber Multiply(ComplexNumber n1, ComplexNumber n2);  
    [OperationContract]  
    ComplexNumber Divide(ComplexNumber n1, ComplexNumber n2);  
}  
```  
  
 `ComplexNumber` 类的公共成员被 <xref:System.Xml.Serialization.XmlSerializer> 序列化为 XML 属性。 <xref:System.Runtime.Serialization.DataContractSerializer> 不能用于创建此类 XML 实例。  
  
```csharp  
public class ComplexNumber  
{  
    private double real;  
    private double imaginary;  
  
    [XmlAttribute]  
    public double Real  
    {  
        get { return real; }  
        set { real = value; }  
    }  
  
    [XmlAttribute]  
    public double Imaginary  
    {  
        get { return imaginary; }  
        set { imaginary = value; }  
    }  
  
    public ComplexNumber(double real, double imaginary)  
    {  
        this.Real = real;  
        this.Imaginary = imaginary;  
    }  
    public ComplexNumber()  
    {  
        this.Real = 0;  
        this.Imaginary = 0;  
    }  
  
}  
```  
  
 服务实现计算并返回相应的结果 - 接受并返回 `ComplexNumber` 类型的值。  
  
```csharp  
public class XmlSerializerCalculatorService : IXmlSerializerCalculator  
{  
    public ComplexNumber Add(ComplexNumber n1, ComplexNumber n2)  
    {  
        return new ComplexNumber(n1.Real + n2.Real, n1.Imaginary +  
                                                      n2.Imaginary);  
    }  
    …  
}  
```  
  
 客户端实现也使用复数。 服务协定和数据类型都在 generatedClient.cs 源文件中定义，该文件是由 "" 元数据实用工具工具生成的，它从服务元数据中 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 。 Svcutil.exe 可以检测何时协定不能由 <xref:System.Runtime.Serialization.DataContractSerializer> 序列化，在这种情况下转而发出 `XmlSerializable` 类型。 如果要强制使用 <xref:System.Xml.Serialization.XmlSerializer>，可以将 /serializer:XmlSerializer（使用 XmlSerializer）命令选项传递给 Svcutil.exe 工具。  
  
```csharp  
// Create a client.  
XmlSerializerCalculatorClient client = new  
                         XmlSerializerCalculatorClient();  
  
// Call the Add service operation.  
ComplexNumber value1 = new ComplexNumber();  
value1.Real = 1;  
value1.Imaginary = 2;  
ComplexNumber value2 = new ComplexNumber();  
value2.Real = 3;  
value2.Imaginary = 4;  
ComplexNumber result = client.Add(value1, value2);  
Console.WriteLine("Add({0} + {1}i, {2} + {3}i) = {4} + {5}i",  
    value1.Real, value1.Imaginary, value2.Real, value2.Imaginary,
    result.Real, result.Imaginary);  
    …  
}  
```  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Add(1 + 2i, 3 + 4i) = 4 + 6i  
Subtract(1 + 2i, 3 + 4i) = -2 + -2i  
Multiply(2 + 3i, 4 + 7i) = -13 + 26i  
Divide(3 + 7i, 5 + -2i) = 0.0344827586206897 + 1.41379310344828i  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\Interop\XmlSerializer`  
