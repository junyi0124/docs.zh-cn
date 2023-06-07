---
title: 如何：在配置中指定服务绑定
description: 了解如何在配置文件中为 WCF 服务配置终结点。 协定是为服务定义的，并在类中实现。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 885037f7-1c2b-4d7a-90d9-06b89be172f2
ms.openlocfilehash: 06b1cd009d28f854ec73286efa29d42f0f557314
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293684"
---
# <a name="how-to-specify-a-service-binding-in-configuration"></a>如何：在配置中指定服务绑定

在本示例中，为基本计算器服务定义 `ICalculator` 协定，在 `CalculatorService` 类中实现该服务，然后在 Web.config 文件中配置其终结点，该文件中指定此服务使用 <xref:System.ServiceModel.BasicHttpBinding>。 有关如何使用代码而不是配置来配置此服务的说明，请参阅 [如何：在代码中指定服务绑定](how-to-specify-a-service-binding-in-code.md)。  
  
 通常，最佳做法是以声明方式在配置中指定绑定和地址信息，而不是在代码中强制指定。 在代码中定义终结点通常是不可行的，因为已部署服务的绑定和地址通常与在部署服务时所用的绑定和地址不同。 一般说来，通过将绑定和寻址信息放置在代码之外，无需重新编译或重新部署应用程序即可更改这些信息。  
  
 可以使用 [配置编辑器工具 ( # A0) ](configuration-editor-tool-svcconfigeditor-exe.md)执行以下所有配置步骤。  
  
 有关此示例的源副本，请参阅 [BasicBinding](./samples/basicbinding.md)。  
  
## <a name="to-specify-the-basichttpbinding-to-use-to-configure-the-service"></a>指定要用于配置服务的 BasicHttpBinding  
  
1. 为该类型的服务定义服务协定。  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#1)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#1)]  
  
2. 在服务类中实现该服务协定。  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#2)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#2)]  
  
    > [!NOTE]
    > 未在该服务的实现内部指定地址或绑定信息。 而且，不必编写码就可以从配置文件中获取该信息。  
  
3. 创建一个 Web.config 文件来配置使用 `CalculatorService` 的 <xref:System.ServiceModel.WSHttpBinding> 的终结点。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name=" CalculatorService" >  

            <!-- Leave the address blank to be populated by default -->
            <!-- from the hosting environment,in this case IIS, so -->
            <!-- the address will just be that of the IIS Virtual -->
            <!-- Directory. -->

            <!-- Specify the binding configuration name for that -->
            <!-- binding type. This is optional but useful if you -->
            <!-- want to modify the properties of the binding. -->
            <!-- The bindingConfiguration name Binding1 is defined -->
            <!-- below in the bindings element. -->
            <endpoint
                address=""
                binding="wsHttpBinding"  
                bindingConfiguration="Binding1"  
                contract="ICalculator" />  
          </service>  
        </services>  
        <bindings>  
          <wsHttpBinding>  
            <binding name="Binding1">  
              <!-- Binding property values can be modified here. -->  
              <!-- See the next procedure. -->  
            </binding>  
          </wsHttpBinding>  
       </bindings>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
4. 创建一个包含下行的 Service.svc 文件，然后将其放到 Internet 信息服务 (IIS) 虚拟目录中。  
  
    ```aspx-csharp
    <%@ServiceHost language=c# Service="CalculatorService" %>
    ```  
  
## <a name="to-modify-the-default-values-of-the-binding-properties"></a>修改绑定属性的默认值  
  
1. 若要修改的默认属性值之一 <xref:System.ServiceModel.WSHttpBinding> ，请在元素中创建一个新的绑定配置名称， `<binding name="Binding1">` [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) 并在此绑定元素中为该绑定的属性设置新值。 例如，若要将默认打开和关闭超时值从 1 分钟更改为 2 分钟，需要将下列内容添加到配置文件中。  
  
    ```xml  
    <wsHttpBinding>  
      <binding name="Binding1"  
               closeTimeout="00:02:00"  
               openTimeout="00:02:00">  
      </binding>  
    </wsHttpBinding>  
    ```  
  
## <a name="see-also"></a>另请参阅

- [使用绑定配置服务和客户端](using-bindings-to-configure-services-and-clients.md)
- [指定终结点地址](specifying-an-endpoint-address.md)
