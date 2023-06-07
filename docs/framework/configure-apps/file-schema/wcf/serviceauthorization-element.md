---
title: <serviceAuthorization> 元素
ms.date: 03/30/2017
ms.assetid: 18cddad5-ddcb-4839-a0ac-1d6f6ab783ca
ms.openlocfilehash: f476f754a340f52859be2986e42754cba0ef3771
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "71834011"
---
# <a name="serviceauthorization-element"></a>\<serviceAuthorization> 元素

指定用于授予服务操作访问权限的设置。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceAuthorization>**  

## <a name="syntax"></a>语法

```xml
<serviceAuthorization impersonateCallerForAllOperations="Boolean"
                      principalPermissionMode="None/UseWindowsGroups/UseAspNetRoles/Custom"
                      roleProviderName="String"
                      serviceAuthorizationManagerType="String">
  <authorizationPolicies>
    <add policyType="String" />
  </authorizationPolicies>
</serviceAuthorization>
```

## <a name="attributes-and-elements"></a>特性和元素

以下各节描述了特性、子元素和父元素：

### <a name="attributes"></a>特性

|属性|说明|  
|---------------|-----------------|  
|impersonateCallerForAllOperations|一个布尔值，指定是否服务中的所有操作都模拟调用方。 默认为 `false`。<br /><br /> 当特定服务操作模拟调用方时，线程上下文会在执行指定服务前切换为调用方上下文。|  
|principalPermissionMode|设置用于在服务器上执行操作的主体。 包括以下值：<br /><br /> -无<br />-UseWindowsGroups<br />-UseAspNetRoles<br />-Custom<br /><br /> 默认值为 UseWindowsGroups。 此值的类型为 <xref:System.ServiceModel.Description.PrincipalPermissionMode>。 有关使用此属性的详细信息，请参阅[如何：使用 PrincipalPermissionAttribute 类限制访问权限](../../../wcf/how-to-restrict-access-with-the-principalpermissionattribute-class.md)。|  
|roleProviderName|一个字符串，指定为 Windows Communication Foundation (WCF) 应用程序提供角色信息的角色提供程序的名称。 默认值为空字符串。|  
|ServiceAuthorizationManagerType|一个包含服务授权管理器的类型的字符串。 有关详细信息，请参阅 <xref:System.ServiceModel.ServiceAuthorizationManager>。|  

### <a name="child-elements"></a>子元素

|元素|描述|  
|-------------|-----------------|  
|authorizationPolicies|包含可使用 `add` 关键字添加的授权策略类型的集合。 每个授权类型都包含一个所需的 `policyType` 属性，此属性是一个字符串。 该属性指定一个授权策略，可以将一组输入声明转换为另一组声明。 可以根据该授权策略来授予或拒绝访问控制。 有关详细信息，请参阅 <xref:System.ServiceModel.Configuration.AuthorizationPolicyTypeElement>。|  

### <a name="parent-elements"></a>父元素

|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|包含服务行为的设置集合。|  

## <a name="remarks"></a>注解

本节包含一些影响授权、自定义角色提供程序和模拟的元素。  
  
`principalPermissionMode` 属性指定在授权使用受保护方法时要使用的用户组。 默认值为 `UseWindowsGroups`，该值指定在 Windows 组（例如，“Administrators”或“Users”）中搜索试图访问某个资源的标识。 你还可以指定 `UseAspNetRoles` 以使用在元素下配置的自定义角色提供程序 \<system.web> ，如以下代码所示：

```xml
<system.web>
  <membership defaultProvider="SqlProvider"
              userIsOnlineTimeWindow="15">
    <providers>
      <clear />
      <add name="SqlProvider"
           type="System.Web.Security.SqlMembershipProvider"
           connectionStringName="SqlConn"
           applicationName="MembershipProvider"
           enablePasswordRetrieval="false"
           enablePasswordReset="false"
           requiresQuestionAndAnswer="false"
           requiresUniqueEmail="true"
           passwordFormat="Hashed" />
    </providers>
  </membership>
  <!-- Other configuration code not shown. -->
</system.web>
```
  
下面的代码演示了 `roleProviderName` 与属性一起使用的 `principalPermissionMode` ：
  
```xml
<behaviors>
  <behavior name="ServiceBehaviour">
    <serviceAuthorization principalPermissionMode ="UseAspNetRoles"
                          roleProviderName ="SqlProvider" />
  </behavior>
  <!-- Other configuration code not shown. -->
</behaviors>
```

有关使用此配置元素的详细示例，请参阅[授权访问服务操作](../../../wcf/samples/authorizing-access-to-service-operations.md)和[授权策略](../../../wcf/samples/authorization-policy.md)。
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Configuration.ServiceAuthorizationElement>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [授予对服务操作的访问权限](../../../wcf/samples/authorizing-access-to-service-operations.md)
- [如何：为服务创建自定义授权管理器](../../../wcf/extending/how-to-create-a-custom-authorization-manager-for-a-service.md)
- [如何：使用 PrincipalPermissionAttribute 类限制访问](../../../wcf/how-to-restrict-access-with-the-principalpermissionattribute-class.md)
- [授权策略](../../../wcf/samples/authorization-policy.md)
