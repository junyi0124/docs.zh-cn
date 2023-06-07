---
title: invalidCERCall MDA
description: 查看 invalidCERCall 托管调试助手 (MDA) ，如果在受约束的执行区域内有无效的调用 (CER) 图形，则激活该操作。
ms.date: 03/30/2017
helpviewer_keywords:
- invalid CER calls
- InvalidCERCall MDA
- MDAs (managed debugging assistants), CER calls
- constrained execution regions
- CER calls
- managed debugging assistants (MDAs), CER calls
ms.assetid: c4577410-602e-44e5-9dab-fea7c55bcdfe
ms.openlocfilehash: 6f0d9d8e3059729098975080224999d0c0fac46e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272652"
---
# <a name="invalidcercall-mda"></a>invalidCERCall MDA

方法没有可靠性协定或具有过弱协定时，如果受约束的执行区域 (CER) 图内存在对此类方法的调用，将激活 `invalidCERCall` 托管调试助手 (MDA)。 弱协定是这样的一种协定：它声明最坏情况下的状态损坏的范围超出传递给调用的实例，即 <xref:System.AppDomain> 或进程状态可能会损坏，或者在 CER 中调用它时不可始终明确地计算其结果。  
  
## <a name="symptoms"></a>症状  

 在 CER 中执行代码时得到意外结果。 这些症状并不明确。 它们可能是在调入不可靠的方法时发生的意外 <xref:System.OutOfMemoryException>、<xref:System.Threading.ThreadAbortException> 或其他异常，因为运行时没有预先准备该方法，或没有提供相应的保护以使它在运行时不引发 <xref:System.Threading.ThreadAbortException> 异常。 更大的威胁在于，该方法在运行时产生的任何异常可能将 <xref:System.AppDomain> 或进程置于不稳定状态，这违背了 CER 的目标。 创建 CER 的目的就是避免这样的状态损坏。 损坏状态的症状特定于应用程序，因为一致状态的定义在应用程序之间并不相同。  
  
## <a name="cause"></a>原因  

 CER 中的代码调用没有 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> 或具有弱 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> 的函数，该函数不适合在 CER 中运行。  
  
 根据可靠性协定语法，弱协定是未指定 <xref:System.Runtime.ConstrainedExecution.Consistency> 枚举值或指定 <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptProcess>、<xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptAppDomain> 或 <xref:System.Runtime.ConstrainedExecution.Cer.None> 的 <xref:System.Runtime.ConstrainedExecution.Consistency> 值的协定。 上述任一条件均指示调用的代码可能妨碍 CER 中的其他代码维护一致状态的工作。  CER 允许代码以非常明确的方式处理错误，维护对应用程序很重要的内部不变量并允许它在面对诸如内存不足异常之类的瞬态错误时继续运行。  
  
 此 MDA 的激活指示一种可能性：CER 中正在调用的方法会以调用方未预期的方式或导致 <xref:System.AppDomain> 进程状态损坏或不可恢复的方式失败。 当然，调用的代码可能正确执行，并且问题只是缺少协定。 但是，编写可靠代码所涉及的是一些细微的问题，缺少协定是代码可能无法正确执行的一个很好的指示器。 协定是程序员已可靠地编写代码并承诺这些保证不会在将来的代码修订中改变的指示器。  也就是说，协定是意图声明，而不只是实现细节。  
  
 由于具有弱协定或不具有协定的任何方法都有可能以多种不可预测的方式失败，运行时并不尝试从方法（例如由惰性 JIT 编译、泛型字典填充或线程中止所引入的方法）中删除它自己的任何不可预测的失败。 也就是说，当此 MDA 被激活时，它指示运行时不在所定义的 CER 中包括调用的方法；调用关系图在此节点终止，因为继续准备此子树将有助于掩蔽潜在的错误。  
  
## <a name="resolution"></a>解决方法  

 向该函数添加有效的可靠性协定，或避免使用该函数调用。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 从 CER 调用弱协定的影响可能是 CER 未能完成其操作。 这可能导致 <xref:System.AppDomain> 进程状态的损坏。  
  
## <a name="output"></a>输出  

 下面是来自此 MDA 的示例输出。  
  
 `Method 'MethodWithCer', while executing within a constrained execution region, makes a call at IL offset 0x000C to 'MethodWithWeakContract', which does not have a sufficiently strong reliability contract and might cause non-deterministic results.`  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidCERCall />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
