---
title: 自定义 WSDL 发布
ms.date: 03/30/2017
ms.assetid: 3b3e8103-2c95-4db3-a05b-46aa8e9d4d29
ms.openlocfilehash: 83377e1c72ef5774c909729abd1312cce5364ab0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262861"
---
# <a name="custom-wsdl-publication"></a>自定义 WSDL 发布

此示例演示了如何执行以下操作：  
  
- 在自定义 <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> 属性 (Attribute) 上实现 <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>，以便将该属性 (Attribute) 的属性 (Property) 导出为 WSDL 批注。  
  
- 实现 <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> 以导入自定义 WSDL 批注。  
  
- 分别在自定义协定行为和自定义操作行为上实现 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension?displayProperty=nameWithType> 和 <xref:System.ServiceModel.Description.IOperationContractGenerationExtension?displayProperty=nameWithType>，以便在 CodeDom 中将导入的批注作为导入的协定和操作的注释写入。  
  
- 使用 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 下载 wsdl，使用 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 自定义 wsdl 导入程序导入 wsdl，并使用 <xref:System.ServiceModel.Description.ServiceContractGenerator?displayProperty=nameWithType> 生成 Windows Communication Foundation (WCF) 客户端代码，并将 wsdl 批注作为///和 c # 中的注释和 Visual Basic。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
## <a name="service"></a>服务  

 此示例中的服务使用两个自定义属性进行标记。 第一个是 `WsdlDocumentationAttribute`，它在构造函数中接受字符串，可用来通过描述用法的字符串提供协定接口或操作。 第二个是 `WsdlParamOrReturnDocumentationAttribute`，可用来返回值或参数，以说明操作中的那些值。 下面的示例演示使用这些属性描述的服务协定 `ICalculator`。  
  
```csharp  
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
// Document it.  
[WsdlDocumentation("The ICalculator contract performs basic calculation services.")]  
public interface ICalculator  
{  
    [OperationContract]  
    [WsdlDocumentation("The Add operation adds two numbers and returns the result.")]  
    [return:WsdlParamOrReturnDocumentation("The result of adding the two arguments together.")]  
    double Add(  
      [WsdlParamOrReturnDocumentation("The first value to add.")]double n1,
      [WsdlParamOrReturnDocumentation("The second value to add.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Subtract operation subtracts the second argument from the first.")]  
    [return:WsdlParamOrReturnDocumentation("The result of the second argument subtracted from the first.")]  
    double Subtract(  
      [WsdlParamOrReturnDocumentation("The value from which the second is subtracted.")]double n1,
      [WsdlParamOrReturnDocumentation("The value that is subtracted from the first.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Multiply operation multiplies two values.")]  
    [return:WsdlParamOrReturnDocumentation("The result of multiplying the first and second arguments.")]  
    double Multiply(  
      [WsdlParamOrReturnDocumentation("The first value to multiply.")]double n1,
      [WsdlParamOrReturnDocumentation("The second value to multiply.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Divide operation returns the value of the first argument divided by the second argument.")]  
    [return:WsdlParamOrReturnDocumentation("The result of dividing the first argument by the second.")]  
    double Divide(  
      [WsdlParamOrReturnDocumentation("The numerator.")]double n1,
      [WsdlParamOrReturnDocumentation("The denominator.")]double n2  
    );  
}  
```  
  
 `WsdlDocumentationAttribute` 实现 <xref:System.ServiceModel.Description.IContractBehavior> 和 <xref:System.ServiceModel.Description.IOperationBehavior>，因此当打开服务时，可以将属性实例添加到对应的 <xref:System.ServiceModel.Description.ContractDescription> 或 <xref:System.ServiceModel.Description.OperationDescription> 中。 该属性还实现 <xref:System.ServiceModel.Description.IWsdlExportExtension>。 调用 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%28System.ServiceModel.Description.WsdlExporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> 时，用来导出元数据的 <xref:System.ServiceModel.Description.WsdlExporter> 以及包含服务说明对象的 <xref:System.ServiceModel.Description.WsdlContractConversionContext> 将作为参数进行传递，以允许修改导出的元数据。  
  
 在此示例中，根据导出上下文对象具有的是 <xref:System.ServiceModel.Description.ContractDescription> 还是 <xref:System.ServiceModel.Description.OperationDescription>，将使用文本属性 (Property) 从该属性 (Attribute) 中提取注释，并将注释添加到 WSDL 批注元素中，如下面的代码所示。  
  
```csharp
public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)
{
    if (contractDescription != null)
    {
        // Inside this block it is the contract-level comment attribute.
        // This.Text returns the string for the contract attribute.
        // Set the doc element; if this isn't done first, there is no XmlElement in the
        // DocumentElement property.
        context.WsdlPortType.Documentation = string.Empty;
        // Contract comments.
        XmlDocument owner = context.WsdlPortType.DocumentationElement.OwnerDocument;
        XmlElement summaryElement = owner.CreateElement("summary");
        summaryElement.InnerText = this.Text;
        context.WsdlPortType.DocumentationElement.AppendChild(summaryElement);
    }
    else
    {
        Operation operation = context.GetOperation(operationDescription);
        if (operation != null)
        {
            // We are dealing strictly with the operation here.
            // This.Text returns the string for the operation-level attributes.
            // Set the doc element; if this isn't done first, there is no XmlElement in the
            // DocumentElement property.
            operation.Documentation = String.Empty;

            // Operation C# triple comments.
            XmlDocument owner = operation.DocumentationElement.OwnerDocument;
            XmlElement newSummaryElement = owner.CreateElement("summary");
            newSummaryElement.InnerText = this.Text;
            operation.DocumentationElement.AppendChild(newSummaryElement);
        }
    }
}
```  
  
 如果导出的是操作，该示例则使用反射来获取参数的任何 `WsdlParamOrReturnDocumentationAttribute` 值和返回值，并将它们添加到该操作的 WSDL 批注元素中，如下所示。  
  
```csharp
// Get returns information  
ParameterInfo returnValue = operationDescription.SyncMethod.ReturnParameter;  
object[] returnAttrs = returnValue.GetCustomAttributes(typeof(WsdlParamOrReturnDocumentationAttribute), false);  
if (returnAttrs.Length != 0)  
{  
    // <returns>text.</returns>  
    XmlElement returnsElement = owner.CreateElement("returns");  
    returnsElement.InnerText = ((WsdlParamOrReturnDocumentationAttribute)returnAttrs[0]).ParamComment;  
    operation.DocumentationElement.AppendChild(returnsElement);  
}  
  
// Get parameter information.  
ParameterInfo[] args = operationDescription.SyncMethod.GetParameters();  
for (int i = 0; i < args.Length; i++)  
{  
    object[] docAttrs = args[i].GetCustomAttributes(typeof(WsdlParamOrReturnDocumentationAttribute), false);  
    if (docAttrs.Length == 1)  
    {  
        // <param name="Int1">Text.</param>  
        XmlElement newParamElement = owner.CreateElement("param");  
        XmlAttribute paramName = owner.CreateAttribute("name");  
        paramName.Value = args[i].Name;  
        newParamElement.InnerText = ((WsdlParamOrReturnDocumentationAttribute)docAttrs[0]).ParamComment;  
        newParamElement.Attributes.Append(paramName);  
        operation.DocumentationElement.AppendChild(newParamElement);  
    }  
}  
```  
  
 该示例然后使用以下配置文件通过标准方式发布元数据。  
  
```xml  
<services>  
  <service
      name="Microsoft.ServiceModel.Samples.CalculatorService"  
      behaviorConfiguration="CalculatorServiceBehavior">  
    <!-- ICalculator is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc  -->  
    <endpoint address=""  
              binding="wsHttpBinding"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    <!-- the mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->  
    <endpoint address="mex"  
              binding="mexHttpBinding"  
              contract="IMetadataExchange" />  
  </service>  
</services>  
  
<!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="False" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
## <a name="svcutil-client"></a>Svcutil 客户端  

 此示例不使用 Svcutil.exe。 协定是在 generatedClient.cs 文件中提供的，因此在该示例演示自定义 WSDL 导入和代码生成后，可以调用服务。 若要在此示例中使用以下自定义 WSDL 导入程序，可以运行 Svcutil.exe 并指定 `/svcutilConfig` 选项，以给出此示例中使用的客户端配置文件的路径（它引用 `WsdlDocumentation.dll` 库）。 但是，若要加载 `WsdlDocumentationImporter`，Svuctil.exe 必须能够定位并加载 `WsdlDocumentation.dll` 库，这意味着它要么在全局程序集缓存中注册，要么在路径中注册，要么与 Svcutil.exe 位于相同的目录中。 对于类似此示例的基本示例而言，最简单的方式是将 Svcutil.exe 和客户端配置文件复制到 `WsdlDocumentation.dll` 所在的目录中，并从该目录中运行它。  
  
## <a name="the-custom-wsdl-importer"></a>自定义 WSDL 导入程序  

 自定义 <xref:System.ServiceModel.Description.IWsdlImportExtension> 对象 `WsdlDocumentationImporter` 还实现要添加到导入的 ServiceEndpoints 中的 <xref:System.ServiceModel.Description.IContractBehavior> 和 <xref:System.ServiceModel.Description.IOperationBehavior>，以及要在创建协定或操作代码时为了修改代码而调用的 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension> 和 <xref:System.ServiceModel.Description.IOperationContractGenerationExtension>。  
  
 首先，在 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> 方法中，该示例确定 WSDL 批注是在协定级别还是操作级别，并在相应的范围将其自身添加为一个行为，从而将导入的批注文本传递给其构造函数。  
  
```csharp
public void ImportContract(WsdlImporter importer, WsdlContractConversionContext context)  
{  
    // Contract Documentation  
    if (context.WsdlPortType.Documentation != null)  
    {  
        // System examines the contract behaviors to see whether any implement IWsdlImportExtension.  
        context.Contract.Behaviors.Add(new WsdlDocumentationImporter(context.WsdlPortType.Documentation));  
    }  
    // Operation Documentation  
    foreach (Operation operation in context.WsdlPortType.Operations)  
    {  
        if (operation.Documentation != null)  
        {  
            OperationDescription operationDescription = context.Contract.Operations.Find(operation.Name);  
            if (operationDescription != null)  
            {  
                // System examines the operation behaviors to see whether any implement IWsdlImportExtension.  
                operationDescription.Behaviors.Add(new WsdlDocumentationImporter(operation.Documentation));  
            }  
        }  
    }  
}  
```  
  
 然后在生成代码时，系统调用 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> 和 <xref:System.ServiceModel.Description.IOperationContractGenerationExtension.GenerateOperation%28System.ServiceModel.Description.OperationContractGenerationContext%29> 方法，以传递相应的上下文信息。 该示例对自定义 WSDL 批注进行格式化，并将它们作为注释插入到 CodeDom 中。  
  
```csharp
public void GenerateContract(ServiceContractGenerationContext context)  
{  
    Debug.WriteLine("In generate contract.");  
    context.ContractType.Comments.AddRange(FormatComments(text));  
}  
  
public void GenerateOperation(OperationContractGenerationContext context)  
{  
    context.SyncMethod.Comments.AddRange(FormatComments(text));  
    Debug.WriteLine("In generate operation.");  
}  
```  
  
## <a name="the-client-application"></a>客户端应用程序  

 客户端应用程序通过在应用程序配置文件中指定自定义 WSDL 导入程序来加载该导入程序。  
  
```xml  
<client>  
  <endpoint address="http://localhost/servicemodelsamples/service.svc"
  binding="wsHttpBinding"
  contract="ICalculator" />  
  <metadata>  
    <wsdlImporters>  
      <extension type="Microsoft.ServiceModel.Samples.WsdlDocumentationImporter, WsdlDocumentation"/>  
    </wsdlImporters>  
  </metadata>  
</client>  
```  
  
 指定自定义导入程序后，WCF 元数据系统会将自定义导入程序加载到 <xref:System.ServiceModel.Description.WsdlImporter> 为该目的创建的任何。 此示例使用 <xref:System.ServiceModel.Description.MetadataExchangeClient> 下载元数据，使用经过正确配置的 <xref:System.ServiceModel.Description.WsdlImporter> 通过此示例创建的自定义导入程序导入元数据，并使用 <xref:System.ServiceModel.Description.ServiceContractGenerator> 将修改后的协定信息编译成 Visual Basic 和 C# 客户端代码（可在 Visual Studio 中使用以支持 Intellisense），或者编译成 XML 文档。  
  
```csharp
/// From WSDL Documentation:  
///
/// <summary>The ICalculator contract performs basic calculation
/// services.</summary>
///
[System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "3.0.0.0")]  
[System.ServiceModel.ServiceContractAttribute(Namespace="http://Microsoft.ServiceModel.Samples", ConfigurationName="ICalculator")]  
public interface ICalculator  
{  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Add operation adds two numbers and returns the
    /// result.</summary><returns>The result of adding the two arguments
    /// together.</returns><param name="n1">The first value to add.</param><param
    /// name="n2">The second value to add.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Add", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/AddResponse")]  
    double Add(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Subtract operation subtracts the second argument from the
    /// first.</summary><returns>The result of the second argument subtracted from the
    /// first.</returns><param name="n1">The value from which the second is
    /// subtracted.</param><param name="n2">The value that is subtracted from the
    /// first.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Subtract", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/SubtractResponse")]  
    double Subtract(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Multiply operation multiplies two values.</summary><returns>The
    /// result of multiplying the first and second arguments.</returns><param
    /// name="n1">The first value to multiply.</param><param name="n2">The second value
    /// to multiply.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Multiply", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/MultiplyResponse")]  
    double Multiply(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Divide operation returns the value of the first argument divided
    /// by the second argument.</summary><returns>The result of dividing the first
    /// argument by the second.</returns><param name="n1">The numerator.</param><param
    /// name="n2">The denominator.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Divide", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/DivideResponse")]  
    double Divide(double n1, double n2);  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Metadata\WsdlDocumentation`  
