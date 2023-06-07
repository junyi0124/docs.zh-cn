---
title: 如何：在企业中锁定终结点
ms.date: 03/30/2017
ms.assetid: 1b7eaab7-da60-4cf7-9d6a-ec02709cf75d
ms.openlocfilehash: 68a7b80a01d9b1a8c5243331e63a1b82996e8ee6
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558142"
---
# <a name="how-to-lock-down-endpoints-in-the-enterprise"></a>如何：在企业中锁定终结点

大企业往往要求开发的应用程序符合企业安全策略。 以下主题讨论如何开发和安装可用于验证计算机上安装的所有 Windows Communication Foundation (WCF) 客户端应用程序的客户端终结点验证程序。

在这种情况下，验证程序是客户端验证程序，因为此终结点行为已添加到 [\<commonBehaviors>](../../configure-apps/file-schema/wcf/commonbehaviors.md) machine.config 文件中的客户端部分。 WCF 只为客户端应用程序加载常见终结点行为，并仅为服务应用程序加载常见服务行为。 若要为服务应用程序安装此相同验证程序，验证程序必须为服务行为。 有关详细信息，请参阅 [\<commonBehaviors>](../../configure-apps/file-schema/wcf/commonbehaviors.md) 部分。

> [!IMPORTANT]
> <xref:System.Security.AllowPartiallyTrustedCallersAttribute> [\<commonBehaviors>](../../configure-apps/file-schema/wcf/commonbehaviors.md) 当应用程序在部分信任环境中运行时，不会运行添加到配置文件的节中的服务或终结点行为 (APTCA) ，而在发生此情况时不会引发异常。 若要强制运行公共行为（如验证程序），必须执行以下任一操作：
>
> - 使用属性标记常见行为， <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 以使其在部署为部分信任应用程序时能够运行。 请注意，可以在计算机上设置注册表项，以防运行标有 APTCA 的程序集。
>
> - 确保将应用程序部署为完全受信任的应用程序，用户不能修改代码访问安全设置以在部分信任环境中运行该应用程序。 如果用户可以这样做，则自定义验证程序不会运行，且不引发任何异常。 若要确保这一点，请参阅 `levelfinal` 使用 [代码访问安全策略工具的选项 ( # A0) ](../../tools/caspol-exe-code-access-security-policy-tool.md)。
>
> 有关详细信息，请参阅 [部分信任的最佳实践](../feature-details/partial-trust-best-practices.md) 和 [支持的部署方案](../feature-details/supported-deployment-scenarios.md)。

### <a name="to-create-the-endpoint-validator"></a>创建终结点验证程序

1. 使用 <xref:System.ServiceModel.Description.IEndpointBehavior> 方法中所需的验证步骤创建一个 <xref:System.ServiceModel.Description.IEndpointBehavior.Validate%2A>。 以下代码是一个示例。  (`InternetClientValidatorBehavior` 从 [安全验证](../samples/security-validation.md) 示例获取。 ) 

    [!code-csharp[LockdownValidation#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/internetclientvalidatorbehavior.cs#2)]

2. 创建用于注册步骤 1 中创建的终结点验证程序的新 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement>。 下面的代码示例演示了此过程。  (此示例的原始代码位于 [安全验证](../samples/security-validation.md) 示例中。 ) 

    [!code-csharp[LockdownValidation#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/internetclientvalidatorelement.cs#3)]

3. 确保用强名称对编译的程序集签名。 有关详细信息，请参阅 [强名称工具 ( # A0) ](../../tools/sn-exe-strong-name-tool.md) 和适用于你的语言的编译器命令。

### <a name="to-install-the-validator-into-the-target-computer"></a>将验证程序安装到目标计算机中

1. 使用恰当的机制安装终结点验证程序。 在企业中，可以使用组策略和 Systems Management Server (SMS)。

2. 使用 [Gacutil.exe (全局程序集缓存工具) ](../../tools/gacutil-exe-gac-tool.md)将强名称程序集安装到全局程序集缓存中。

3. 使用 <xref:System.Configuration?displayProperty=nameWithType> 命名空间类型执行以下操作：

    1. [\<behaviorExtensions>](../../configure-apps/file-schema/wcf/behaviorextensions.md)使用完全限定的类型名称将扩展名添加到部分，并锁定该元素。

         [!code-csharp[LockdownValidation#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#5)]

    2. 将行为元素添加到 `EndpointBehaviors` 部分的属性中 [\<commonBehaviors>](../../configure-apps/file-schema/wcf/commonbehaviors.md) ，并锁定该元素。  (若要在服务上安装验证程序，验证程序必须是 <xref:System.ServiceModel.Description.IServiceBehavior> 并添加到 `ServiceBehaviors` 属性中。 ) 下面的代码示例显示了步骤 a 后的正确配置。 和 b. 之后的适当配置，唯一例外是有没有强名称。

        [!code-csharp[LockdownValidation#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#6)]

    3. 保存 machine.config 文件。 下面的代码示例执行步骤 3 中的所有任务，但在本地保存已修改的 machine.config 文件的副本。

        [!code-csharp[LockdownValidation#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#7)]

## <a name="example"></a>示例

下面的代码示例演示如何将公共行为添加到 machine.config 文件并将副本保存到磁盘。 `InternetClientValidatorBehavior`取自[安全验证](../samples/security-validation.md)示例。

[!code-csharp[LockdownValidation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/lockdownvalidation/cs/hostapplication.cs#1)]

## <a name="net-framework-security"></a>.NET Framework 安全性

您可能还需要对配置文件元素进行加密。 有关更多信息，请参见“另请参见”部分。

## <a name="see-also"></a>请参阅

- [使用 DPAPI 对配置文件元素进行加密](/previous-versions/msp-n-p/ff647398(v=pandp.10))
- [使用 RSA 对配置文件元素进行加密](/previous-versions/msp-n-p/ff650304(v=pandp.10))
