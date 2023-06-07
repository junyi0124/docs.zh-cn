---
title: 如何：为服务创建自定义授权管理器
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Communication Foundation, extending
- OperationRequirement class
ms.assetid: 6214afde-44c1-4bf5-ba07-5ad6493620ea
ms.openlocfilehash: 9c28cb81b78f80505cfcf5f7e4dfdba083bd0793
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70797107"
---
# <a name="how-to-create-a-custom-authorization-manager-for-a-service"></a>如何：为服务创建自定义授权管理器

Windows Communication Foundation （WCF）中的标识模型基础结构支持基于声明的可扩展授权模型。 声明是从令牌中提取的，自定义授权策略将有选择地对其进行处理，然后放入 <xref:System.IdentityModel.Policy.AuthorizationContext> 中。 授权管理器检查 <xref:System.IdentityModel.Policy.AuthorizationContext> 中的声明，从而作出授权决策。

默认情况下，授权决策由 <xref:System.ServiceModel.ServiceAuthorizationManager> 类作出；但是，可通过创建自定义授权管理器来覆盖这些决策。 若要创建自定义授权管理器，请创建一个从 <xref:System.ServiceModel.ServiceAuthorizationManager> 派生的类，并实现 <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> 方法。 授权决策是在 <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> 方法中作出的，如果允许访问，该方法返回 `true`，如果拒绝访问，则返回 `false`。

如果授权决策取决于消息正文的内容，请使用 <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccess%2A> 方法。

由于性能问题，如有可能，应重新设计应用程序，使授权决策不需要访问消息正文。

在代码或配置中，可以进行服务的自定义授权管理器注册。

### <a name="to-create-a-custom-authorization-manager"></a>创建自定义授权管理器

1. 从 <xref:System.ServiceModel.ServiceAuthorizationManager> 类派生一个类。

    [!code-csharp[c_CustomAuthMgr#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customauthmgr/cs/c_customauthmgr.cs#5)]
    [!code-vb[c_CustomAuthMgr#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customauthmgr/vb/c_customauthmgr.vb#5)]

2. 重写 <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%28System.ServiceModel.OperationContext%29> 方法。

    使用传给 <xref:System.ServiceModel.OperationContext> 方法的 <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%28System.ServiceModel.OperationContext%29> 进行授权决策。

    下面的代码示例使用 <xref:System.IdentityModel.Claims.ClaimSet.FindClaims%28System.String%2CSystem.String%29> 方法查找自定义声明 `http://www.contoso.com/claims/allowedoperation`，从而做出授权决策。

    [!code-csharp[c_CustomAuthMgr#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customauthmgr/cs/c_customauthmgr.cs#6)]
    [!code-vb[c_CustomAuthMgr#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customauthmgr/vb/c_customauthmgr.vb#6)]

### <a name="to-register-a-custom-authorization-manager-using-code"></a>使用代码注册自定义授权管理器

1. 创建自定义授权管理器的一个实例，然后将其分配给 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ServiceAuthorizationManager%2A> 属性。

    使用 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> 属性可以访问 <xref:System.ServiceModel.ServiceHostBase.Authorization%2A>。

    下面的代码示例注册 `MyServiceAuthorizationManager` 自定义授权管理器。

    [!code-csharp[c_CustomAuthMgr#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customauthmgr/cs/c_customauthmgr.cs#4)]
    [!code-vb[c_CustomAuthMgr#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customauthmgr/vb/c_customauthmgr.vb#4)]

### <a name="to-register-a-custom-authorization-manager-using-configuration"></a>使用配置注册自定义授权管理器

1. 打开服务的配置文件。

2. 将 serviceAuthorization > 添加到[行为>\<](../../configure-apps/file-schema/wcf/behaviors.md)。 [ \<](../../configure-apps/file-schema/wcf/serviceauthorization-element.md)

    `serviceAuthorizationManagerType` [对于\<serviceAuthorization >](../../configure-apps/file-schema/wcf/serviceauthorization-element.md)，添加一个属性，并将其值设置为表示自定义授权管理器的类型。

3. 添加一个保护客户端和服务之间的通信的绑定。

    为此通信选择的绑定决定了添加到 <xref:System.IdentityModel.Policy.AuthorizationContext> 的声明，自定义授权管理器使用这些声明来进行授权决策。 有关系统提供的绑定的更多详细信息，请参阅[系统提供的绑定](../system-provided-bindings.md)。

4. 通过将[ \<服务 >](../../configure-apps/file-schema/wcf/service.md)元素添加到服务终结点，并将该`behaviorConfiguration`属性的值设置为[ \<行为 >](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md)元素的 name 属性的值，将该行为关联到服务终结点。

    有关配置服务终结点的详细信息，请[参阅如何：在配置](../feature-details/how-to-create-a-service-endpoint-in-configuration.md)中创建服务终结点。

    下面的代码示例注册自定义授权管理器 `Samples.MyServiceAuthorizationManager`。

    ```xml
    <configuration>
      <system.serviceModel>
        <services>
          <service
              name="Microsoft.ServiceModel.Samples.CalculatorService"
              behaviorConfiguration="CalculatorServiceBehavior">
            <host>
              <baseAddresses>
                <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
              </baseAddresses>
            </host>
            <endpoint address=""
                      binding="wsHttpBinding_Calculator"
                      contract="Microsoft.ServiceModel.Samples.ICalculator" />
          </service>
        </services>
        <bindings>
          <WSHttpBinding>
           <binding name = "wsHttpBinding_Calculator">
             <security mode="Message">
               <message clientCredentialType="Windows"/>
             </security>
            </binding>
          </WSHttpBinding>
        </bindings>
        <behaviors>
          <serviceBehaviors>
            <behavior name="CalculatorServiceBehavior">
              <serviceAuthorization serviceAuthorizationManagerType="Samples.MyServiceAuthorizationManager,MyAssembly" />
             </behavior>
         </serviceBehaviors>
       </behaviors>
      </system.serviceModel>
    </configuration>
    ```

    > [!WARNING]
    > 请注意，当您指定了 serviceAuthorizationManagerType 时，字符串必须包含完全限定的类型名称、 一个逗号，以及在其中定义类型的程序集的名称。 如果您忽略了程序集名称，WCF 会尝试从 System.ServiceModel.dll 加载类型。

## <a name="example"></a>示例

下面的代码示例演示 <xref:System.ServiceModel.ServiceAuthorizationManager> 类的一个基本实现，包括重写 <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> 方法。 该示例代码检查某个自定义声明的 <xref:System.IdentityModel.Policy.AuthorizationContext>，如果该自定义声明的资源与 `true` 中的操作值匹配，则返回 <xref:System.ServiceModel.OperationContext>。 有关更完整的<xref:System.ServiceModel.ServiceAuthorizationManager>类实现，请参阅[授权策略](../samples/authorization-policy.md)。

[!code-csharp[c_CustomAuthMgr#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customauthmgr/cs/c_customauthmgr.cs#2)]
[!code-vb[c_CustomAuthMgr#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customauthmgr/vb/c_customauthmgr.vb#2)]

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.ServiceAuthorizationManager>
- [授权策略](../samples/authorization-policy.md)
