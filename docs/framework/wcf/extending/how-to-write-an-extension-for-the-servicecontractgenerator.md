---
title: 如何：编写 ServiceContractGenerator 的扩展
ms.date: 03/30/2017
ms.assetid: 876ca823-bd16-4bdf-9e0f-02092df90e51
ms.openlocfilehash: 3a81204ca470b9b94d9e8f048ecccbade74b2e10
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254631"
---
# <a name="how-to-write-an-extension-for-the-servicecontractgenerator"></a>如何：编写 ServiceContractGenerator 的扩展

本主题描述如何编写 <xref:System.ServiceModel.Description.ServiceContractGenerator> 的扩展。 这可以通过对操作行为实现 <xref:System.ServiceModel.Description.IOperationContractGenerationExtension> 接口或者对协定行为实现 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension> 接口来完成。 本主题演示如何对协定行为实现 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension> 接口。  
  
 <xref:System.ServiceModel.Description.ServiceContractGenerator> 从 <xref:System.ServiceModel.Description.ServiceEndpoint>、<xref:System.ServiceModel.Description.ContractDescription> 和 <xref:System.ServiceModel.Channels.Binding> 实例生成服务协定、客户端类型和客户端配置。 通常，需要先从服务元数据导入 <xref:System.ServiceModel.Description.ServiceEndpoint>、<xref:System.ServiceModel.Description.ContractDescription> 和 <xref:System.ServiceModel.Channels.Binding> 实例，然后使用这些实例生成调用服务的代码。 在本示例中，<xref:System.ServiceModel.Description.IWsdlImportExtension> 实现用于处理 WSDL 批注，然后将代码生成扩展添加到导入的协定中，以生成对已生成代码的注释。  
  
### <a name="to-write-an-extension-for-the-servicecontractgenerator"></a>编写 ServiceContractGenerator 的扩展  
  
1. 实现 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension>。 若要修改已生成的服务协定，请使用传入 <xref:System.ServiceModel.Description.ServiceContractGenerationContext> 方法的 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> 实例。  
  
    ```csharp
    public void GenerateContract(ServiceContractGenerationContext context)  
    {  
        Console.WriteLine("In generate contract.");  
        context.ContractType.Comments.AddRange(Formatter.FormatComments(commentText));  
    }  
    ```  
  
2. 在同一个类上实现 <xref:System.ServiceModel.Description.IWsdlImportExtension>。 通过将代码生成扩展添加到导入的 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> 实例，<xref:System.ServiceModel.Description.ContractDescription> 方法可以处理特定的 WSDL 扩展（此例中为 WSDL 批注）。  
  
    ```csharp
    public void ImportContract(WsdlImporter importer, WsdlContractConversionContext context)
    {
        // Contract documentation
        if (context.WsdlPortType.Documentation != null)
        {
            context.Contract.Behaviors.Add(new WsdlDocumentationImporter(context.WsdlPortType.Documentation));
        }
        // Operation documentation
        foreach (Operation operation in context.WsdlPortType.Operations)
        {
            if (operation.Documentation != null)
            {
                OperationDescription operationDescription = context.Contract.Operations.Find(operation.Name);
                if (operationDescription != null)
                {
                    operationDescription.Behaviors.Add(new WsdlDocumentationImporter(operation.Documentation));
                }
            }
        }
    }
    public void BeforeImport(ServiceDescriptionCollection wsdlDocuments, XmlSchemaSet xmlSchemas, ICollection<XmlElement> policy)
    {
        Console.WriteLine("BeforeImport called.");
    }

    public void ImportEndpoint(WsdlImporter importer, WsdlEndpointConversionContext context)
    {
        Console.WriteLine("ImportEndpoint called.");
    }
    ```  
  
3. 将 WSDL 导入程序添加到客户端配置中。  
  
    ```xml  
    <metadata>  
      <wsdlImporters>  
        <extension type="Microsoft.WCF.Documentation.WsdlDocumentationImporter, WsdlDocumentation" />  
      </wsdlImporters>  
    </metadata>  
    ```  
  
4. 在客户端代码中，创建一个 `MetadataExchangeClient` 并调用 `GetMetadata`。  
  
    ```csharp
    var mexClient = new MetadataExchangeClient(metadataAddress);  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet metaDocs = mexClient.GetMetadata();  
    ```  
  
5. 创建一个 `WsdlImporter` 并调用 `ImportAllContracts`。  
  
    ```csharp
    var importer = new WsdlImporter(metaDocs);
    System.Collections.ObjectModel.Collection<ContractDescription> contracts = importer.ImportAllContracts();  
    ```  
  
6. 创建一个 `ServiceContractGenerator` 并为每个协定调用 `GenerateServiceContractType`。  
  
    ```csharp
    var generator = new ServiceContractGenerator();  
    foreach (ContractDescription contract in contracts)  
    {  
       generator.GenerateServiceContractType(contract);  
    }  
    if (generator.Errors.Count != 0)  
       throw new Exception("There were errors during code compilation.");  
    ```  
  
7. 会为实现 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> 的给定协定上的每个协定行为自动调用 <xref:System.ServiceModel.Description.IServiceContractGenerationExtension>。 然后，此方法可以修改传入的 <xref:System.ServiceModel.Description.ServiceContractGenerationContext>。 在此示例中，添加了注释。  
  
## <a name="see-also"></a>另请参阅

- Metadata 
- [如何：导入自定义 WSDL](how-to-import-custom-wsdl.md)
