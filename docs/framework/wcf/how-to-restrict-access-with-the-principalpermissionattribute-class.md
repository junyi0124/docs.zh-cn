---
title: 如何：使用 PrincipalPermissionAttribute 类限制访问
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PrincipalPermissionAttribute class
- WCF, authorization
- WCF, security
ms.assetid: 5162f5c4-8781-4cc4-9425-bb7620eaeaf4
ms.openlocfilehash: 92d27548c510a19bf36ffaffb532f48461146d99
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96269608"
---
# <a name="how-to-restrict-access-with-the-principalpermissionattribute-class"></a>如何：使用 PrincipalPermissionAttribute 类限制访问

控制对 Windows 域计算机上的资源的访问是一项基本的安全任务。 例如，应该只有某些用户才能查看敏感数据，例如工资单信息。 本主题解释如何通过要求用户属于某个预定义组来限制对方法的访问。 有关工作示例，请参阅 [授权访问服务操作](./samples/authorizing-access-to-service-operations.md)。  
  
 此任务由两个单独的过程组成。 第一个过程创建组，并用用户填充它。 第二个过程应用 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 类以指定组。  
  
### <a name="to-create-a-windows-group"></a>创建 Windows 组  
  
1. 打开 " **计算机管理** " 控制台。  
  
2. 在左侧面板中，单击 " **本地用户和组**"。  
  
3. 右键单击 " **组**"，然后单击 " **新建组**"。  
  
4. 在 " **组名称** " 框中，键入新组的名称。  
  
5. 在 " **说明** " 框中，键入新组的描述。  
  
6. 单击 " **添加** " 按钮将新成员添加到组中。  
  
7. 如果你已经将自己添加到该组中，并且想要测试下面的代码，则必须注销计算机并重新登录，以便将自己包括到该组中。  
  
### <a name="to-demand-user-membership"></a>要求用户具有成员资格  
  
1. 打开包含已实现的服务协定代码 (WCF) 代码文件的 Windows Communication Foundation。 有关实现协定的详细信息，请参阅 [实现服务协定](implementing-service-contracts.md)。  
  
2. 将 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 属性应用于每个必须限制到特定组的方法。 将 <xref:System.Security.Permissions.SecurityAttribute.Action%2A> 属性设置为 <xref:System.Security.Permissions.SecurityAction.Demand>，并将 <xref:System.Security.Permissions.PrincipalPermissionAttribute.Role%2A> 属性设置为组名称。 例如：  
  
     [!code-csharp[c_PrincipalPermissionAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_principalpermissionattribute/cs/source.cs#1)]
     [!code-vb[c_PrincipalPermissionAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_principalpermissionattribute/vb/source.vb#1)]  
  
    > [!NOTE]
    > 如果将 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 属性应用到某个协定，将会引发 <xref:System.Security.SecurityException>。 只能在方法级别应用该属性。  
  
## <a name="using-a-certificate-to-control-access-to-a-method"></a>使用证书控制对方法的访问  

 如果客户端凭据类型是证书，还可以使用 `PrincipalPermissionAttribute` 类控制对方法的访问。 为此，您必须具有证书的主题和指纹。  
  
 若要检查证书的属性，请参阅 [如何：使用 MMC 管理单元查看证书](./feature-details/how-to-view-certificates-with-the-mmc-snap-in.md)。 若要查找 "指纹" 值，请参阅 [如何：检索证书的指纹](./feature-details/how-to-retrieve-the-thumbprint-of-a-certificate.md)。  
  
#### <a name="to-control-access-using-a-certificate"></a>使用证书控制访问  
  
1. 将 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 类应用于您要对其访问进行限制的方法。  
  
2. 将属性的操作设置为 <xref:System.Security.Permissions.SecurityAction.Demand?displayProperty=nameWithType>。  
  
3. 将 `Name` 属性设置为包含主题名称和证书指纹的字符串。 用一个分号和一个空格将这两个值分隔开，如下面的示例所示：  
  
     [!code-csharp[c_PrincipalPermissionAttribute#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_principalpermissionattribute/cs/source.cs#2)]
     [!code-vb[c_PrincipalPermissionAttribute#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_principalpermissionattribute/vb/source.vb#2)]  
  
4. 将 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A> 属性设置为 <xref:System.ServiceModel.Description.PrincipalPermissionMode.UseAspNetRoles>，如下面的配置示例所示：  
  
    ```xml  
    <behaviors>  
      <serviceBehaviors>  
      <behavior name="SvcBehavior1">  
      <serviceAuthorization principalPermissionMode="UseAspNetRoles" />  
      </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
     将此值设置为 `UseAspNetRoles` 表示 `Name` 的 `PrincipalPermissionAttribute` 属性将用于执行字符串比较。 使用证书作为客户端凭据时，默认情况下，WCF 会将证书公用名和指纹连接起来，以创建客户端主要标识的唯一值。 在将 `UseAspNetRoles` 设置为服务上的 `PrincipalPermissionMode` 之后，会将此主标识值与 `Name` 属性值进行比较，以确定用户的访问权限。  
  
     此外，在创建自承载服务时，还可以按照以下代码中的方式在代码中设置 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A> 属性：  
  
     [!code-csharp[c_PrincipalPermissionAttribute#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_principalpermissionattribute/cs/source.cs#3)]
     [!code-vb[c_PrincipalPermissionAttribute#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_principalpermissionattribute/vb/source.vb#3)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Security.Permissions.PrincipalPermissionAttribute>
- <xref:System.Security.Permissions.SecurityAction.Demand>
- <xref:System.Security.Permissions.PrincipalPermissionAttribute.Role%2A>
- [授予对服务操作的访问权限](./samples/authorizing-access-to-service-operations.md)
- [安全性概述](./feature-details/security-overview.md)
- [实现服务协定](implementing-service-contracts.md)
