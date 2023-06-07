---
title: 如何：在可靠会话内保护消息
ms.date: 03/30/2017
ms.assetid: aee33e50-936f-4486-9ca8-c1520c19a62d
ms.openlocfilehash: cec9356467886be022d05ead55d5cb6ccddcd838
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558675"
---
# <a name="how-to-secure-messages-within-reliable-sessions"></a>如何：在可靠会话内保护消息

本主题概述了使用系统提供的绑定之一来启用在可靠会话内交换的消息的消息级安全所需的步骤。这些绑定支持这种会话，但默认情况下不支持。 通过使用代码或在配置文件中以声明方式强制启用安全、可靠的会话。 此过程使用客户端和服务配置文件来启用安全的可靠会话。

此过程由以下三个关键任务组成：

1. 指定客户端和服务在可靠会话内交换消息。

1. 在可靠会话内要求消息级安全。

1. 指定客户端必须用来向服务进行身份验证的客户端凭据类型。

在第一个任务中，终结点配置元素包含一个 `bindingConfiguration` 属性，该属性引用名为 (的绑定配置，在此示例中) `MessageSecurity` 。 [**\<binding>**](../../configure-apps/file-schema/wcf/bindings.md)然后，配置元素引用此名称，通过将元素的属性设置 `enabled` [**\<reliableSession>**](/previous-versions/ms731375(v=vs.90)) 为来启用可靠会话 `true` 。 通过将 `ordered` 属性设置为 `true`，可以要求可靠会话内的有序传送保证。

对于此配置过程所基于的示例的源副本，请参阅 [WS 可靠会话](../samples/ws-reliable-session.md)。

第二个任务的基本项是通过将 `mode` **\<security>** 客户端和服务的元素中包含的元素的属性设置 **\<binding>** 为来完成的 `Message` 。

第三项任务的基本项是通过将 `clientCredentialType` **\<message>** 客户端和服务的元素中包含的元素的属性设置 **\<security>** 为来完成的 `Certificate` 。

> [!NOTE]
> 如果在可靠会话中使用消息安全，则在发生超时而不是在第一次失败时引发异常的情况下，可靠的消息传送将尝试对未经身份验证的客户端

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>使用 WSHttpBinding 配置服务以使用可靠会话

[如何：在可靠会话内交换消息](how-to-exchange-messages-within-a-reliable-session.md)中介绍了此过程。

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>使用 WSHttpBinding 配置客户端以使用可靠会话

[如何：在可靠会话内交换消息](how-to-exchange-messages-within-a-reliable-session.md)中介绍了此过程。

### <a name="set-the-mode-and-clientcredentialtype-in-configuration"></a>在配置中设置模式和 ClientCredentialType

1. 将相应的绑定元素添加到 [**\<bindings>**](../../configure-apps/file-schema/wcf/bindings.md) 配置文件的元素中。 下面的示例添加一个 [**\<wsHttpBinding>**](../../configure-apps/file-schema/wcf/wshttpbinding.md) 元素。

1. 添加 **\<binding>** 元素，并将其 `name` 属性设置为合适的值。 该示例使用名称 `MessageSecurity` 。

1. 添加一个 **\<security>** 元素，并将 `mode` 属性设置为 `Message` 。

1. 在 **\<security>** 元素中，添加一个 **\<message>** 元素，并将 `clientCredentialType` 特性设置为 `Certificate` 。

```xml
<wsHttpBinding>
  <binding name="MessageSecurity">
    <security mode="Message">
      <message clientCredentialType="Certificate" />
    </security>
  </binding>
</wsHttpBinding>
```
