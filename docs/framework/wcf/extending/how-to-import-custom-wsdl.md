---
title: 如何：导入自定义 WSDL
ms.date: 03/30/2017
ms.assetid: ddc3718d-ce60-44f6-92af-a5c67477dd99
ms.openlocfilehash: c5aa554394743314a91afd6a5cdf86f9974e81f8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249080"
---
# <a name="how-to-import-custom-wsdl"></a>如何：导入自定义 WSDL

本主题描述如何导入自定义 WSDL。 若要处理自定义 WSDL，必须实现 <xref:System.ServiceModel.Description.IWsdlImportExtension> 接口。  
  
### <a name="to-import-custom-wsdl"></a>导入自定义 WSDL  
  
1. 实现 <xref:System.ServiceModel.Description.IWsdlImportExtension>。 实现 <xref:System.ServiceModel.Description.IWsdlImportExtension.BeforeImport%28System.Web.Services.Description.ServiceDescriptionCollection%2CSystem.Xml.Schema.XmlSchemaSet%2CSystem.Collections.Generic.ICollection%7BSystem.Xml.XmlElement%7D%29> 方法，以便在导入元数据之前对其进行修改。 实现 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportEndpoint%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlEndpointConversionContext%29> 和 <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> 方法，以便修改从元数据导入的协定和终结点。 若要访问所导入的协定或终结点，请使用相应的上下文对象（<xref:System.ServiceModel.Description.WsdlContractConversionContext> 或 <xref:System.ServiceModel.Description.WsdlEndpointConversionContext>）：  
  
    ```csharp
    public class WsdlDocumentationImporter : IWsdlImportExtension
    {
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
    }
    ```
  
2. 将客户端应用程序配置为使用自定义 WSDL 导入程序。 请注意，如果使用的是 Svcutil.exe，则应将此配置添加到 Svcutil.exe (Svcutil.exe.config) 的配置文件中：  
  
    ```xml  
    <system.serviceModel>  
          <client>  
            <endpoint
              address="http://localhost:8000/Fibonacci"
              binding="wsHttpBinding"  
              contract="IFibonacci"  
            />  
            <metadata>  
              <wsdlImporters>  
                <extension type="Microsoft.WCF.Documentation.WsdlDocumentationImporter, WsdlDocumentation" />  
              </wsdlImporters>  
            </metadata>  
          </client>  
        </system.serviceModel>  
    ```  
  
3. 创建一个新的 <xref:System.ServiceModel.Description.WsdlImporter> 实例（并将其传入包含要导入的 WSDL 文档的 <xref:System.ServiceModel.Description.MetadataSet> 实例），然后调用 <xref:System.ServiceModel.Description.WsdlImporter.ImportAllContracts%2A>：  
  
    ```csharp
    WsdlImporter importer = new WsdlImporter(metaDocs);
    System.Collections.ObjectModel.Collection<ContractDescription> contracts = importer.ImportAllContracts();  
    ```  
  
## <a name="see-also"></a>另请参阅

- Metadata 
- [导出和导入元数据](../feature-details/exporting-and-importing-metadata.md)
- [自定义 WSDL 发布](../samples/custom-wsdl-publication.md)
