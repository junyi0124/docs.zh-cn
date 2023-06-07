---
title: 如何：创建自定义声明
description: 了解如何在 WCF 中创建自定义声明。 WCF 支持各种内置声明，某些应用程序可能需要自定义声明。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d619976b-eda3-475e-ac23-c7988a2dceb0
ms.openlocfilehash: ea3bc7384ca10538ca5ab1d3bb05da6a2757fb67
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256009"
---
# <a name="how-to-create-a-custom-claim"></a>如何：创建自定义声明

Windows Communication Foundation 中的标识模型基础结构 (WCF) 提供一组内置声明类型和权限，以及用于创建 <xref:System.IdentityModel.Claims.Claim> 具有这些类型和权限的实例的帮助器函数。 这些内置声明旨在为 WCF 在默认情况下支持的客户端凭据类型中找到的信息建模。 在许多情况下，这些内置的声明足够满足需要，然而一些应用程序可能需要自定义声明。 声明由声明类型、要应用该声明的资源和在该资源上断言的权限组成。 本主题描述如何创建自定义声明。  
  
### <a name="to-create-a-custom-claim-that-is-based-on-a-primitive-data-type"></a>创建基于基元数据类型的自定义声明  
  
1. 通过将声明类型、资源值和权限传递到 <xref:System.IdentityModel.Claims.Claim.%23ctor%28System.String%2CSystem.Object%2CSystem.String%29> 构造函数来创建自定义声明。  
  
    1. 确定声明类型的唯一值。  
  
         声明类型是一个唯一的字符串标识符。 自定义声明的设计者负责确保声明类型所使用的字符串标识符是唯一的。 有关 WCF 定义的声明类型的列表，请参见 <xref:System.IdentityModel.Claims.ClaimTypes> 类。  
  
    2. 选择资源的基元数据类型和值。  
  
         资源是一个对象。 资源的 CLR 类型可以是一个基元类型，例如 <xref:System.String> 或 <xref:System.Int32>，也可以是任何可序列化的类型。 资源的 CLR 类型必须是可序列化的，因为声明由 WCF 在不同点进行序列化。 基元类型是可序列化的类型。  
  
    3. 选择由 WCF 定义的权限或自定义权限的唯一值。  
  
         权限是一个唯一的字符串标识符。 WCF 定义的权限是在类中定义的 <xref:System.IdentityModel.Claims.Rights> 。  
  
         自定义声明的设计者负责确保权限所使用的字符串标识符是唯一的。  
  
         下面的代码示例创建了一个声明类型为 `http://example.org/claims/simplecustomclaim`、资源名称为 `Driver's License` 并具有 <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> 权限的自定义声明。  
  
     [!code-csharp[c_CustomClaim#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#4)]
     [!code-vb[c_CustomClaim#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#4)]  
  
### <a name="to-create-a-custom-claim-that-is-based-on-a-non-primitive-data-type"></a>创建基于非基元数据类型的自定义声明  
  
1. 通过将声明类型、资源值和权限传递到 <xref:System.IdentityModel.Claims.Claim.%23ctor%28System.String%2CSystem.Object%2CSystem.String%29> 构造函数来创建自定义声明。  
  
    1. 确定声明类型的唯一值。  
  
         声明类型是一个唯一的字符串标识符。 自定义声明的设计者负责确保声明类型所使用的字符串标识符是唯一的。 有关 WCF 定义的声明类型的列表，请参见 <xref:System.IdentityModel.Claims.ClaimTypes> 类。  
  
    2. 为资源选择或定义一个可序列化的非基元类型。  
  
         资源是一个对象。 资源的 CLR 类型必须是可序列化的，因为声明由 WCF 在不同点进行序列化。 基元类型本身是可序列化的类型。  
  
         在定义一个新类型时，请将 <xref:System.Runtime.Serialization.DataContractAttribute> 应用于类。 另外，请将 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性应用于新类型的需要作为声明的一部分序列化的所有成员。  
  
         下面的代码示例定义了一个名为 `MyResourceType` 的自定义资源类型。  
  
         [!code-csharp[c_CustomClaim#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#2)]
         [!code-vb[c_CustomClaim#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#2)]
  
    3. 选择由 WCF 定义的权限或自定义权限的唯一值。  
  
         权限是一个唯一的字符串标识符。 WCF 定义的权限是在类中定义的 <xref:System.IdentityModel.Claims.Rights> 。  
  
         自定义声明的设计者负责确保权限所使用的字符串标识符是唯一的。  
  
         下面的代码示例创建了一个声明类型为 `http://example.org/claims/complexcustomclaim`、自定义资源类型为 `MyResourceType` 并具有 <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> 权限的自定义声明。  
  
         [!code-csharp[c_CustomClaim#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#5)]
         [!code-vb[c_CustomClaim#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#5)]
  
## <a name="example"></a>示例  

 下面的代码示例演示如何创建一个具有基元资源类型的自定义声明和一个具有非基元资源类型的自定义声明。  
  
 [!code-csharp[c_CustomClaim#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaim/cs/c_customclaim.cs#0)]
 [!code-vb[c_CustomClaim#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaim/vb/c_customclaim.vb#0)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Claims.Claim>
- <xref:System.IdentityModel.Claims.Rights>
- <xref:System.IdentityModel.Claims.ClaimTypes>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [使用标识模型管理声明和授权](../feature-details/managing-claims-and-authorization-with-the-identity-model.md)
