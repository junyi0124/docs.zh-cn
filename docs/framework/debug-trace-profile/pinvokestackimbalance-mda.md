---
title: pInvokeStackImbalance MDA
description: 查看 PInvokeStackImbalance MDA，此 MDA 可能在执行或遵循平台调用时在访问冲突或内存损坏期间被激活。
ms.date: 03/30/2017
helpviewer_keywords:
- signatures, platform invoke
- stack depth
- platform invoke stack imbalance
- MDAs (managed debugging assistants), platform invoke
- platform invoke, run-time errors
- PInvokeStackImbalance MDA
- managed debugging assistants (MDAs), platform invoke
ms.assetid: 34ddc6bd-1675-4f35-86aa-de1645d5c631
ms.openlocfilehash: 89afd3fce3f2a8bffe88d45991ceeb59fc5e5b76
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803659"
---
# <a name="pinvokestackimbalance-mda"></a>PInvokeStackImbalance MDA

`PInvokeStackImbalance`当 CLR 检测到平台调用之后的堆栈深度与预期的堆栈深度不匹配时，将激活托管调试助手（MDA），前提是属性中指定的调用约定 <xref:System.Runtime.InteropServices.DllImportAttribute> 和托管签名中参数的声明。

仅为 32 位 x86 平台实现 `PInvokeStackImbalance` MDA。

> [!NOTE]
> `PInvokeStackImbalance`默认情况下，禁用 MDA。 在 Visual Studio 2017 及更高版本中， `PInvokeStackImbalance` MDA 出现在 "**异常设置**" 对话框中的 "**托管调试助手**" 列表中（当您选择 "**调试**  >  **Windows**  >  **异常设置**" 时，将显示此对话框）。 但是，如果选中或清除 "**引发时中断**" 复选框，则不会启用或禁用 MDA;它仅控制在激活 MDA 时 Visual Studio 是否引发异常。

## <a name="symptoms"></a>症状

进行平台 invoke 调用时或之后，应用程序会遭遇访问冲突或内存损坏。

## <a name="cause"></a>原因

平台 invoke 调用的托管签名可能不匹配正在被调用的方法的非托管签名。  这种不匹配可能是由于没有声明参数的正确数量或没有指定参数的适当大小的托管签名所导致的。  MDA 也可因调用约定（可能由 <xref:System.Runtime.InteropServices.DllImportAttribute> 属性指定）不匹配非托管调用约定而激活。

## <a name="resolution"></a>解决方法

检查托管平台调用签名和调用约定，以确认它与本机目标的签名和调用约定匹配。  请尝试在托管端和非托管端上显式指定调用约定。 非托管函数也可能（尽管可能性不太大）出于某种原因（例如非托管编译器中的 bug）使堆栈不平衡。

## <a name="effect-on-the-runtime"></a>对运行时的影响

强制所有平台 invoke 调用都采用 CLR 中的非优化路径。

## <a name="output"></a>Output

MDA 消息会提供正导致堆栈不平衡的平台 invoke 方法调用的名称。 方法 `SampleMethod` 上的平台 invoke 调用的示例消息为：

**对 PInvoke 函数 "SampleMethod" 的调用与堆栈不平衡。这很可能是因为托管 PInvoke 签名与非托管目标签名不匹配。检查 PInvoke 签名的调用约定和参数是否与目标非托管签名匹配。**

## <a name="configuration"></a>Configuration

```xml
<mdaConfig>
  <assistants>
    <pInvokeStackImbalance />
  </assistants>
</mdaConfig>
```

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
