---
title: 声明创建和资源值
ms.date: 03/30/2017
helpviewer_keywords:
- claims [WCF], creation and resource values
ms.assetid: 30431f76-cbe7-4bad-bad7-8e43e23a82d4
ms.openlocfilehash: c3f36d607d88b208753066dcbd4e9baa6a2590fa
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295127"
---
# <a name="claim-creation-and-resource-values"></a>声明创建和资源值

<xref:System.IdentityModel.Claims.Claim> 类提供了多种创建内置声明类型的实例的方法。 在这些方法中，以下方法不对提供的资源执行语义或格式检查：  
  
- <xref:System.IdentityModel.Claims.Claim.CreateDnsClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateHashClaim%2A>（不检查字节数组的长度或内容）  
  
- <xref:System.IdentityModel.Claims.Claim.CreateNameClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateSpnClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateThumbprintClaim%2A>（不检查字节数组的长度或内容）  
  
- <xref:System.IdentityModel.Claims.Claim.CreateUpnClaim%2A>  
  
 调用上述方法时应该小心，确保传入的资源值使用正确的格式和/或包含正确的信息类型。  
  
 以下方法采用特定类型：  
  
- <xref:System.IdentityModel.Claims.Claim.CreateDenyOnlyWindowsSidClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateMailAddressClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateRsaClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateUriClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateWindowsSidClaim%2A>  
  
- <xref:System.IdentityModel.Claims.Claim.CreateX500DistinguishedNameClaim%2A>  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Claims.Claim>
- <xref:System.IdentityModel.Claims.ClaimSet>
- [使用标识模型管理声明和授权](managing-claims-and-authorization-with-the-identity-model.md)
