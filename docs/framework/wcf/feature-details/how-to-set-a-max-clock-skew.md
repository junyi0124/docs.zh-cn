---
title: 如何：设置最大时钟偏差
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MaxClockSkew property
- WCF, custom bindings
ms.assetid: 491d1705-eb29-43c2-a44c-c0cf996f74eb
ms.openlocfilehash: 8dd38f3d07773a4be67648b9c1830206438200d6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242872"
---
# <a name="how-to-set-a-max-clock-skew"></a>如何：设置最大时钟偏差

如果两台计算机上的时钟设置不同，时间关键函数可能无法正常执行。 若要减小这种可能性，可以将 `MaxClockSkew` 属性设置为一个 <xref:System.TimeSpan>。 可在两个类上获得此属性：  
  
 <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>  
  
 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>  
  
> [!IMPORTANT]
> 对于安全对话，在 `MaxClockSkew` 引导服务或客户端时必须进行对属性的更改。 为此，必须在属性返回的上设置属性 <xref:System.ServiceModel.Channels.SecurityBindingElement> <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters.BootstrapSecurityBindingElement%2A?displayProperty=nameWithType> 。  
  
 若要更改系统提供的绑定之一上的属性，必须在绑定集合中找到安全绑定元素，然后将 `MaxClockSkew` 属性设置为一个新值。 两个类派生自 <xref:System.ServiceModel.Channels.SecurityBindingElement>: <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 和 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>。 从该集合中检索安全绑定时，必须将其强制转换为上述类型之一，以便正确设置 `MaxClockSkew` 属性。 下面的示例使用了一个 <xref:System.ServiceModel.WSHttpBinding>，它使用了 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>。 有关指定要在每个系统提供的绑定中使用哪种类型的安全绑定的列表，请参阅 [系统提供的绑定](../system-provided-bindings.md)。  
  
## <a name="to-create-a-custom-binding-with-a-new-clock-skew-value-in-code"></a>在代码中使用新的时钟偏差值创建自定义绑定  
  
> [!WARNING]
> 在代码中添加对以下命名空间的引用： <xref:System.ServiceModel.Channels> 、 <xref:System.ServiceModel.Description> 、 <xref:System.Security.Permissions> 和 <xref:System.ServiceModel.Security.Tokens> 。  
  
1. 创建 <xref:System.ServiceModel.WSHttpBinding> 类的一个实例，并将其安全模式设置为 <xref:System.ServiceModel.SecurityMode.Message?displayProperty=nameWithType>。  
  
2. 调用 <xref:System.ServiceModel.Channels.BindingElementCollection> 方法，以此创建 <xref:System.ServiceModel.WSHttpBinding.CreateBindingElements%2A> 的一个新实例。  
  
3. 使用 <xref:System.ServiceModel.Channels.BindingElementCollection.Find%2A> 类的 <xref:System.ServiceModel.Channels.BindingElementCollection> 方法来查找安全绑定元素。  
  
4. 使用 <xref:System.ServiceModel.Channels.BindingElementCollection.Find%2A> 方法时，将其强制转换为实际类型。 下面的示例将其强制转换为 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 类型。  
  
5. 设置安全绑定元素上的 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.MaxClockSkew%2A> 属性。  
  
6. 使用适当的服务类型和基址创建一个 <xref:System.ServiceModel.ServiceHost>。  
  
7. 使用 <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> 方法添加一个终结点并包含该 <xref:System.ServiceModel.Channels.CustomBinding>。  
  
     [!code-csharp[c_MaxClockSkew#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_maxclockskew/cs/source.cs#1)]
     [!code-vb[c_MaxClockSkew#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_maxclockskew/vb/source.vb#1)]  
  
## <a name="to-set-the-maxclockskew-in-configuration"></a>在配置中设置 MaxClockSkew  
  
1. [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md)在 [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 元素部分中创建。  
  
2. 创建一个 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 元素，并将 `name` 属性设置为合适的值。 下面的示例将其设置为 `MaxClockSkewBinding`。  
  
3. 添加一个编码元素。 下面的示例将添加一个 [\<textMessageEncoding>](../../configure-apps/file-schema/wcf/textmessageencoding.md) 。  
  
4. 添加 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 元素，并将 `authenticationMode` 属性设置为适当的设置。 下面的示例将该属性设置为 `Kerberos`，以指定服务使用 Windows 身份验证。  
  
5. 添加 [\<localServiceSettings>](../../configure-apps/file-schema/wcf/localservicesettings-element.md) ，并将 `maxClockSkew` 属性设置为形式的值 `"##:##:##"` 。 下面的示例将其设置为 7 分钟。 还可以添加， [\<localServiceSettings>](../../configure-apps/file-schema/wcf/localservicesettings-element.md) 并将 `maxClockSkew` 属性设置为适当的设置。  
  
6. 添加一个传输元素。 下面的示例使用 [\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md) 。  
  
7. 对于安全对话，安全设置必须出现在元素中的启动时 [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="MaxClockSkewBinding">  
            <textMessageEncoding />  
            <security authenticationMode="Kerberos">  
               <localClientSettings maxClockSkew="00:07:00" />  
               <localServiceSettings maxClockSkew="00:07:00" />  
               <secureConversationBootstrap>  
                  <localClientSettings maxClockSkew="00:30:00" />  
                  <localServiceSettings maxClockSkew="00:30:00" />  
               </secureConversationBootstrap>  
            </security>  
            <httpTransport />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
