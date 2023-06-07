---
title: 如何：将 ASP.NET 角色提供程序与服务一起使用
ms.date: 03/30/2017
ms.assetid: 88d33a81-8ac7-48de-978c-5c5b1257951e
ms.openlocfilehash: 45eeda046e877b4379d7d0e5edd90fac305f5e44
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595292"
---
# <a name="how-to-use-the-aspnet-role-provider-with-a-service"></a>如何：将 ASP.NET 角色提供程序与服务一起使用

ASP.NET 角色提供程序（与 ASP.NET 成员资格提供程序结合使用）是一项功能，使 ASP.NET 开发人员能够创建允许用户通过网站创建帐户并为其分配角色进行授权的网站。 借助此功能，任何用户都可以通过网站建立帐户，并登录以获取该网站及其服务的独占访问权。 这与要求用户在 Windows 域中具有帐户的 Windows 安全完全不同。 相反，任何提供凭据（用户名/密码组合）的用户都可以使用该网站及其服务。  
  
有关示例应用程序，请参阅[成员身份和角色提供程序](../samples/membership-and-role-provider.md)。 有关 ASP.NET 成员资格提供程序功能的详细信息，请参阅[如何：使用 ASP.NET 成员资格提供程序](how-to-use-the-aspnet-membership-provider.md)。  
  
角色提供程序功能使用 SQL Server 数据库存储用户信息。 Windows Communication Foundation （WCF）开发人员可以出于安全目的利用这些功能。 当集成到 WCF 应用程序中时，用户必须为 WCF 客户端应用程序提供用户名/密码组合。 若要使 WCF 能够使用数据库，必须创建类的实例 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> ，将其 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A> 属性设置为 <xref:System.ServiceModel.Description.PrincipalPermissionMode.UseAspNetRoles> ，并将该实例添加到承载服务的的行为集合 <xref:System.ServiceModel.ServiceHost> 。  
  
## <a name="configure-the-role-provider"></a>配置角色提供程序  
  
1. 在 web.config 文件中的 < > 元素下， `system.web` 添加一个 < `roleManager` > 元素，并将其属性设置 `enabled` 为 `true` 。  
  
2. 将 `defaultProvider` 属性设置为 `SqlRoleProvider`。  
  
3. 作为 <> 元素的子 `roleManager` 元素，请添加 <`providers`> 元素。  
  
4. 作为 <> 元素的子 `providers` 元素，添加一个 <> 元素，并将 `add` 以下属性设置为适当的值： `name` 、 `type` 、 `connectionStringName` 和 `applicationName` ，如下面的示例中所示。  
  
    ```xml  
    <!-- Configure the Sql Role Provider. -->  
    <roleManager enabled ="true"
     defaultProvider ="SqlRoleProvider" >  
       <providers>  
         <add name ="SqlRoleProvider"
           type="System.Web.Security.SqlRoleProvider"
           connectionStringName="SqlConn"
           applicationName="MembershipAndRoleProviderSample"/>  
       </providers>  
    </roleManager>  
    ```  
  
## <a name="configure-the-service-to-use-the-role-provider"></a>将服务配置为使用角色提供程序  
  
1. 在 web.config 文件中，添加一个 [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) 元素。  
  
2. 将 [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) 元素添加到 <`system.ServiceModel`> 元素。  
  
3. 将添加 [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) 到 <`behaviors`> 元素。  
  
4. 添加 [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) 元素，并将 `name` 属性设置为合适的值。  
  
5. 将添加 [\<serviceAuthorization>](../../configure-apps/file-schema/wcf/serviceauthorization-element.md) 到 <`behavior`> 元素。  
  
6. 将 `principalPermissionMode` 属性设置为 `UseAspNetRoles`。  
  
7. 将 `roleProviderName` 属性设置为 `SqlRoleProvider`。 下面的示例演示此配置的一个片段。  
  
    ```xml  
    <behaviors>  
     <serviceBehaviors>  
      <behavior name="CalculatorServiceBehavior">  
       <serviceAuthorization principalPermissionMode ="UseAspNetRoles"  
                             roleProviderName ="SqlRoleProvider" />  
      </behavior>  
     </serviceBehaviors>  
    </behaviors>  
    ```  
  
## <a name="see-also"></a>另请参阅

- [成员资格和角色提供程序](../samples/membership-and-role-provider.md)
- [如何：使用 ASP.NET 成员资格提供程序](how-to-use-the-aspnet-membership-provider.md)
