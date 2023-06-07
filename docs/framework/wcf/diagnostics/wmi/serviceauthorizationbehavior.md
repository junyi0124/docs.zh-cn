---
title: ServiceAuthorizationBehavior
ms.date: 03/30/2017
ms.assetid: 77dad8e8-fea4-4d1c-b366-2f01a2a87f78
ms.openlocfilehash: d3625865484568746888ef0638d9a8501e610bef
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273199"
---
# <a name="serviceauthorizationbehavior"></a>ServiceAuthorizationBehavior

ServiceAuthorizationBehavior  
  
## <a name="syntax"></a>语法  
  
```csharp
class ServiceAuthorizationBehavior : Behavior  
{  
  boolean ImpersonateCallerForAllOperations;  
  string PrincipalPermissionMode;  
  string RoleProvider;  
  string ServiceAuthorizationManager;  
};  
```  
  
## <a name="methods"></a>方法  

 ServiceAuthorizationBehavior 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 ServiceAuthorizationBehavior 类具有下列属性：  
  
### <a name="impersonatecallerforalloperations"></a>ImpersonateCallerForAllOperations  

 数据类型：Boolean  
  
 访问类型：只读  
  
 一个值，控制服务是否尝试使用传入消息所提供的凭据进行模拟。  
  
### <a name="principalpermissionmode"></a>PrincipalPermissionMode  

 数据类型：字符串  
  
 访问类型：只读  
  
 用于在服务器上执行操作的主体。  
  
### <a name="roleprovider"></a>RoleProvider  

 数据类型：字符串  
  
 访问类型：只读  
  
 ASP.NET 角色提供程序的名称。  
  
### <a name="serviceauthorizationmanager"></a>ServiceAuthorizationManager  

 数据类型：字符串  
  
 访问类型：只读  
  
 用于自定义授权的授权管理器。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
