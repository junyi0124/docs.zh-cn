---
title: 如何：比较声明
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- claims [WCF], comparing
- claims [WCF]
ms.assetid: 0c4ec84d-53df-408f-8953-9bc437f56c28
ms.openlocfilehash: 29254bd661e72b926b21695ccb646480c53b5475
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70797097"
---
# <a name="how-to-compare-claims"></a>如何：比较声明

Windows Communication Foundation （WCF）中的标识模型基础结构用于执行授权检查。 因此，一个常见的任务是将授权上下文中的声明与执行请求的操作或访问请求的资源所需的声明进行比较。 本主题说明如何比较声明，包括内置和自定义声明类型。 有关标识模型基础结构的详细信息，请参阅[通过标识模型管理声明和授权](../feature-details/managing-claims-and-authorization-with-the-identity-model.md)。

声明比较包括将一个声明的三个部分（类型、权限和资源）与另一个声明中的相同部分进行比较，以确定它们是否相等。 请参见以下示例。

[!code-csharp[c_CustomClaimComparison#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#9)]
[!code-vb[c_CustomClaimComparison#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#9)]

两个声明都具有 <xref:System.IdentityModel.Claims.ClaimTypes.Name%2A> 声明类型、<xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> 权限和字符串“someone”资源。 由于声明的三个部分都相等，因此声明本身也是相等的。

内置的声明类型通过 <xref:System.IdentityModel.Claims.Claim.Equals%2A> 方法进行比较。 必要时使用特定于声明的比较代码。 例如，给定以下两个用户主要名称 (UPN) 声明：

[!code-csharp[c_CustomClaimComparison#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#4)]
[!code-vb[c_CustomClaimComparison#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#4)]

此<xref:System.IdentityModel.Claims.Claim.Equals%2A>方法中的比较代码返回`true`，假设`example\someone`将同一个域用户标识为someone@example.com""。

还可以使用 <xref:System.IdentityModel.Claims.Claim.Equals%2A> 方法比较自定义声明类型。 但是，在声明的 <xref:System.IdentityModel.Claims.Claim.Resource%2A> 属性返回的类型不是基元类型的情况下，只有当 <xref:System.IdentityModel.Claims.Claim.Equals%2A> 属性返回的值依据 `true` 方法相等时，`Resource` 才会返回 <xref:System.IdentityModel.Claims.Claim.Equals%2A>。 当此情况不适用时，`Resource` 属性返回的自定义类型应该重写 <xref:System.IdentityModel.Claims.Claim.Equals%2A> 和 <xref:System.Object.GetHashCode%2A> 方法，以执行任何必要的自定义处理。

## <a name="comparing-built-in-claims"></a>比较内置声明

1. 给定 <xref:System.IdentityModel.Claims.Claim> 类的两个实例，使用 <xref:System.IdentityModel.Claims.Claim.Equals%2A> 进行比较，如下面的代码所示。

     [!code-csharp[c_CustomClaimComparison#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#5)]
     [!code-vb[c_CustomClaimComparison#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#5)]

### <a name="comparing-custom-claims-with-primitive-resource-types"></a>比较具有基元资源类型的自定义声明

1. 对于具有基元资源类型的自定义声明，可以按照内置声明那样的方式执行比较，如下面的代码所示。

     [!code-csharp[c_CustomClaimComparison#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#6)]
     [!code-vb[c_CustomClaimComparison#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#6)]

2. 对于具有基于结构或类的资源类型的自定义声明，资源类型应重写 <xref:System.IdentityModel.Claims.Claim.Equals%2A> 方法。

3. 首先检查 `obj` 参数是否为 `null`，如果是，则返回 `false`。

     [!code-csharp[c_CustomClaimComparison#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#7)]
     [!code-vb[c_CustomClaimComparison#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#7)]

4. 接下来调用 <xref:System.Object.ReferenceEquals%2A>，并将 `this` 和 `obj` 作为参数传递。 如果它返回 `true`，则返回 `true`。

     [!code-csharp[c_CustomClaimComparison#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#8)]
     [!code-vb[c_CustomClaimComparison#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#8)]

5. 然后尝试将 `obj` 分配给一个类类型的局部变量。 如果此操作失败，则引用为 `null`。 在这种情况下返回 `false`。

6. 执行将当前声明与提供的声明进行正确比较所需的自定义比较。

## <a name="example"></a>示例

下面的示例显示自定义声明的比较，其中声明资源是非基元类型。

[!code-csharp[c_CustomClaimComparison#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customclaimcomparison/cs/c_customclaimcomparison.cs#0)]
[!code-vb[c_CustomClaimComparison#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customclaimcomparison/vb/source.vb#0)]

## <a name="see-also"></a>请参阅

- [使用标识模型管理声明和授权](../feature-details/managing-claims-and-authorization-with-the-identity-model.md)
- [如何：创建自定义声明](how-to-create-a-custom-claim.md)
