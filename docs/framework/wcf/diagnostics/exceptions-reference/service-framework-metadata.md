---
title: 服务框架元数据
ms.date: 03/30/2017
ms.assetid: 76afc73a-0770-4084-93f3-6701a757911e
ms.openlocfilehash: 4133e758cde79294432ca501a10ed7ff20821954
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255762"
---
# <a name="service-framework-metadata"></a>服务框架元数据

本主题列出由服务框架元数据生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|AsyncEndCalledOnWrongChannel|在错误的通道上调用异步 End。|  
|AsyncEndCalledWithAnIAsyncResult|使用其他 Begin 方法的 IAsyncResult 调用异步 End。|  
|AttemptedToGetContractTypeForButThatTypeIs1|尝试获取指定的协定类型。 该类型不是 ServiceContract，并且不继承 ServiceContract。|  
|CannotHaveTwoOperationsWithTheSameName3|同一个协定中不能存在两个名称相同的操作， 指定类型的指定方法违反了此规则。 可以通过更改方法名称或使用 OperationContractAttribute 的 Name 属性更改其中一个操作的名称。|  
|CannotInheritTwoOperationsWithTheSameName3|无法继承名称相同的两个不同操作， 指定约定中的指定操作违反了此规则。 可以通过更改方法名称或使用 OperationContractAttribute 的 Name 属性更改其中一个操作的名称。|  
|CantCreateChannelWithManualAddressing|无法为需要请求/答复的协定和需要手动寻址但只支持双工通信的绑定创建通道。|  
|DuplicateBehavior1|该值无法添加到集合中， 因为该集合已经包含一个具有相同指定类型的项。 此集合仅支持每种类型的一个实例。|  
|InAContractInheritanceHierarchyIfParentHasCallbackChildMustToo|因为指定的基本服务协定具有指定的回调协定，所以指定的派生服务协定还必须将指定类型或派生类型指定为其回调协定。|  
|InvalidAsyncBeginMethodSignatureForMethod2|指定 ServiceContract 类型中指定方法的异步 Begin 方法签名无效。 Begin 方法必须采用 AsyncCallback 和一个对象作为最后两个自变量并返回 IAsyncResult。|  
|InvalidAsyncEndMethodSignatureForMethod2|指定 ServiceContract 类型中指定方法的异步 End 方法签名无效。 End 方法必须采用 IAsyncResult 作为最后一个参数。|  
|MessagePropertiesArraySize0|传递的数组没有足够空间容纳此集合包含的全部属性。|  
|OneWayAndFaultsIncompatible2|将指定类型中的指定方法标记为 IsOneWay=true 并且声明了一个或多个 FaultContractAttributes。 单向方法不能声明 FaultContractAttributes。 若要修复此问题，请将 IsOneWay 更改为 false 或者移除 FaultContractAttributes。|  
|UnsupportedWSDLOnlyOneMessage|不支持 WSDL， 仅支持错误消息的一个消息部分。 此错误消息引用了多个消息部分。 如果你具有对 WSDL 文件进行编辑的访问权限，则可以通过删除额外的消息部分解决该问题，以使错误消息仅引用一个部分。|  
|UnsupportedWSDLTheFault|不支持 WSDL， 错误消息部分必须引用某一元素。 此错误消息未引用元素。 如果您具有对 WSDL 文档进行编辑的访问权限，则可以通过使用“element”属性引用架构元素来解决该问题。|  
|WsdlImportErrorDependencyDetail|导入另一个指定值所依赖的指定值时出错。 还指定了 Xpath。|  
|XsdMissingRequiredAttribute1|缺少指定的必需属性。|
