---
title: WCF 配置架构
ms.date: 03/30/2017
ms.assetid: c282aeb5-91f0-4522-8e2f-704c1ef3651f
ms.openlocfilehash: 44d5e0acc6f5a9ca43949bce0c7964354ad18270
ms.sourcegitcommit: 665f8fc55258356f4d2f4a6585b750c974b26675
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2020
ms.locfileid: "91573652"
---
# <a name="wcf-configuration-schema"></a>WCF 配置架构

Windows Communication Foundation (WCF) 配置元素，你可以配置 WCF 服务和客户端应用程序。 可以使用[配置编辑器工具 (SvcConfigEditor.exe)](../../../wcf/configuration-editor-tool-svcconfigeditor-exe.md) 创建和修改客户端和服务的配置文件。 由于配置文件的格式都是以 XML 形式设置的，因此，如果要使用文本编辑器手动编辑这些文件，则您必须熟悉 XML。 否则，您可能会遇到一些问题，如找不到某个 XML 元素标记或特性。 这是因为 XML 元素标记和特性是区分大小写的。  
  
 WCF 配置系统基于 <xref:System.Configuration> 命名空间。 因此，您可以使用 <xref:System.Configuration> 命名空间提供的所有标准功能（如配置锁定、加密和合并）以提高应用程序及其配置的安全性。 有关这些概念的更多信息，请参见下列主题。  
  
 [加密配置信息](/previous-versions/aspnet/53tyfkaw(v=vs.100))  
  
 [锁定配置设置](/previous-versions/aspnet/55th21y4(v=vs.100))  
  
 本节描述每个配置项的所有可能的值，以及它如何与其他 WCF 配置元素进行交互。 下图说明了 WCF 配置架构：

:::image type="content" source="./media/index/windows-communication-foundation-configuration-schema.gif" alt-text="显示 WCF 配置架构的关系图。" lightbox="./media/index/windows-communication-foundation-configuration-schema.gif":::
  
> [!CAUTION]
> 使用适当的访问控制)  (列表 ( # A0) 保护应用程序配置文件中的 WCF 配置节，以避免任何潜在的安全威胁。 例如，确保只有适当的人员可以访问或修改应用程序绑定的安全设置或服务的配置文件的服务模型部分。  
  
## <a name="in-this-section"></a>本节内容  

 [\<system.serviceModel>](system-servicemodel.md)  
 描述 `ServiceModel` 元素。  
  
 [\<system.serviceModel.activation>](system-servicemodel-activation.md)  
 配置 SMSvcHost.exe 工具。  
  
 [\<system.runtime.serialization>](system-runtime-serialization.md)  
 当使用序列化程序（如 <xref:System.Runtime.Serialization.DataContractSerializer>）时，用于设置选项的顶级元素。  
  
## <a name="related-sections"></a>相关章节  

 [配置 Windows Communication Foundation 应用程序](../../../wcf/configuring-services.md)  
 介绍如何配置 WCF 服务和客户端。
