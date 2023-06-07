---
title: 如何：导人自定义策略断言
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1f41d787-accb-4a10-bfc6-a807671d1581
ms.openlocfilehash: fb5e3ba5faca1b32eef63ac174bcd7731ee50771
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249145"
---
# <a name="how-to-import-custom-policy-assertions"></a>如何：导人自定义策略断言

策略断言说明服务终结点的功能和要求。  客户端应用程序可以在服务元数据中使用策略断言来配置客户端绑定或自定义服务终结点的服务协定。  
  
 自定义策略断言可通过实现 <xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType> 接口并将该对象传递给元数据系统或通过在应用程序配置文件中注册实现类型来导入。  接口的实现 <xref:System.ServiceModel.Description.IPolicyImportExtension> 必须提供无参数的构造函数。  
  
### <a name="to-import-custom-policy-assertions"></a>导入自定义策略断言  
  
1. 在类上实现 <xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType> 接口。 请参见下面的过程。  
  
2. 通过以下方式之一插入自定义策略导入程序：  
  
3. 使用配置文件。 请参见下面的过程。  
  
4. 将配置文件与配置的 [元数据实用工具工具一起使用 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)。 请参见下面的过程。  
  
5. 以编程方式插入策略导入程序。 请参见下面的过程。  
  
### <a name="to-implement-the-systemservicemodeldescriptionipolicyimportextension-interface-on-any-class"></a>在任何类上实现 System.ServiceModel.Description.IPolicyImportExtension 接口  
  
1. 在 <xref:System.ServiceModel.Description.IPolicyImportExtension.ImportPolicy%2A?displayProperty=nameWithType> 方法中，对于感兴趣的每个策略主题，通过在传递给该方法的 <xref:System.ServiceModel.Description.PolicyConversionContext?displayProperty=nameWithType> 对象上调用相应方法（具体取决于所需的断言范围），查找要导入的策略断言。 下面的代码示例演示如何使用 <xref:System.ServiceModel.Description.PolicyAssertionCollection.Remove%2A?displayProperty=nameWithType> 方法定位自定义策略断言并在一个步骤中将该断言从集合中删除。 如果使用删除方法定位并删除断言，则不必执行步骤 4。  
  
     [!code-csharp[CustomPolicySample#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/policyimporter.cs#9)]
     [!code-vb[CustomPolicySample#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/custompolicysample/vb/policyimporter.vb#9)]  
  
2. 处理策略断言。 请注意，策略系统不会标准化嵌入的策略和 `wsp:optional`。 您必须在策略导入扩展实现中处理这些结构。  
  
3. 对支持策略断言指定的功能或需求的绑定或协定执行自定义。 断言通常指示绑定需要特定配置或特定绑定元素。 可以通过访问 <xref:System.ServiceModel.Description.PolicyConversionContext.BindingElements%2A?displayProperty=nameWithType> 属性来进行这些修改。 其他断言需要您修改协定。  可以使用 <xref:System.ServiceModel.Description.PolicyConversionContext.Contract%2A?displayProperty=nameWithType> 属性来访问并修改该协定。  请注意，对于同一个绑定和协定，可能会多次调用策略导入程序，但导入的是不同的替代策略（如果一个替代策略导入失败）。 你的代码应该能够处理这一行为。  
  
4. 从断言集合中删除自定义策略断言。 如果不删除断言 Windows Communication Foundation (WCF) 会假定策略导入不成功且不导入关联的绑定。 如果您使用 <xref:System.ServiceModel.Description.PolicyAssertionCollection.Remove%2A?displayProperty=nameWithType> 方法定位自定义策略断言并在一个步骤中将该断言从集合中删除，则不必执行此步骤。  
  
### <a name="to-insert-the-custom-policy-importer-into-the-metadata-system-using-a-configuration-file"></a>使用配置文件将自定义策略导入程序插入到元数据系统  
  
1. 将导入程序类型添加到 `<extensions>` [\<policyImporters>](../../configure-apps/file-schema/wcf/policyimporters.md) 客户端配置文件中元素内的元素中。  
  
     [!code-xml[CustomPolicySample#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/client.exe.config#7)]
  
2. 在客户端应用程序中，使用 <xref:System.ServiceModel.Description.MetadataResolver?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 解析元数据，这会自动调用导入程序。  
  
     [!code-csharp[CustomPolicySample#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/client.cs#10)]
     [!code-vb[CustomPolicySample#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/custompolicysample/vb/client.vb#10)]  
  
### <a name="to-insert-the-custom-policy-importer-into-the-metadata-system-using-svcutilexe"></a>使用 Svcutil.exe 将自定义策略导入程序插入到元数据系统  
  
1. 将导入程序类型添加到 `<extensions>` [\<policyImporters>](../../configure-apps/file-schema/wcf/policyimporters.md) Svcutil.exe.config 配置文件中元素内的元素中。 也可以通过使用 `/svcutilConfig` 选项指示 Svcutil.exe 加载在其他配置文件中注册的策略导入程序类型。  
  
2. 使用工作 [元数据实用工具工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 导入元数据，并自动调用导入程序。  
  
### <a name="to-insert-the-custom-policy-importer-into-the-metadata-system-programmatically"></a>以编程方式将自定义策略导入程序插入到元数据系统  
  
1. 导入元数据之前，将导入程序添加到 <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A?displayProperty=nameWithType> 属性（例如，如果您使用的是 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType>）。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.MetadataResolver?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MetadataResolver?displayProperty=nameWithType>
- [扩展元数据系统](extending-the-metadata-system.md)
