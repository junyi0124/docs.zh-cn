---
title: 当 Web 场中承载 WCF 服务时防止重放攻击
ms.date: 03/30/2017
ms.assetid: 1c2ed695-7a31-4257-92bd-9e9731b886a5
ms.openlocfilehash: 23d0ce32ea4c2450d669b8ca9d887a633f0d99ea
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244777"
---
# <a name="preventing-replay-attacks-when-a-wcf-service-is-hosted-in-a-web-farm"></a>当 Web 场中承载 WCF 服务时防止重放攻击

当使用消息安全性时，WCF 通过从传入的消息创建 NONCE 并检查内部 `InMemoryNonceCache` 以查看是否存在生成的 NONCE，从而防止重播攻击。 如果存在，则该消息作为重播被丢弃。 当 WCF 服务承载在 Web 场中时，因为不跨 Web 场中的节点共享 `InMemoryNonceCache`，所以服务易于受重播攻击。  为了缓解这种情况，WCF 4.5 提供了一个可扩展点，使您能够通过从抽象类 <xref:System.ServiceModel.Security.NonceCache> 中派生类来实现您自己的共享 NONCE 缓存。  
  
## <a name="noncecache-implementation"></a>NonceCache 实现  

 若要实现您自己的共享 NONCE 缓存，请从 <xref:System.ServiceModel.Security.NonceCache> 派生类并重写 <xref:System.ServiceModel.Security.NonceCache.CheckNonce%2A> 和 <xref:System.ServiceModel.Security.NonceCache.TryAddNonce%2A> 方法。 <xref:System.ServiceModel.Security.NonceCache.CheckNonce%2A> 将检查缓存中是否存在指定的 NONCE。 <xref:System.ServiceModel.Security.NonceCache.TryAddNonce%2A> 将尝试向缓存添加 NONCE。 一旦实现类，您就可以通过对实例进行实例化并将其分配给 <xref:System.ServiceModel.Channels.LocalClientSecuritySettings.NonceCache%2A> 来进行客户端重播检测，并分配给  <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.NonceCache%2A> 以进行服务器端重播检测，从而将其挂钩。 没有支持此功能的现成配置。  
  
## <a name="see-also"></a>另请参阅

- [消息安全](message-security-in-wcf.md)
- [SymmetricSecurityBindingElement](../diagnostics/wmi/symmetricsecuritybindingelement.md)
