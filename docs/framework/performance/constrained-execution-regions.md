---
title: 受约束的执行区域
description: 开始使用受约束的执行区域 (CER) ，这是用于创作可靠托管代码的机制的一部分。
ms.date: 03/30/2017
helpviewer_keywords:
- constrained execution regions
- CERs
ms.assetid: 99354547-39c1-4b0b-8553-938e8f8d1808
ms.openlocfilehash: 5014885e186b1fff16543c09d5652958f2463e3d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266852"
---
# <a name="constrained-execution-regions"></a>受约束的执行区域

受约束的执行区域 (CER) 是创建可靠托管代码机制的一部分。 CER 定义一个区域，该区域内公共语言运行时 (CLR) 会受到约束，不能引发阻止该区域内代码完全执行的带外异常。 在该区域中，用户代码受到约束，不能执行会导致引发带外异常的代码。 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 方法应该立即先于 `try` 块执行并将 `catch`、`finally`、`fault` 块标记为受约束的执行区域。 标记为受约束的区域后，代码只能调用其他具有强可靠性协定的代码，并且代码不应对未准备好或不可靠的方法进行分配和虚拟调用，除非该代码已准备好处理失败。 CLR 会为 CER 中正在执行的代码延迟线程中止。  

> [!IMPORTANT]
> 仅 .NET Framework 支持 CER。 本文不适用于 .NET Core 或 .NET 5 及更高版本。

 除已批注的 `try` 块外，受约束的执行区域还以其他形式用于 CLR 中，特别是在 <xref:System.Runtime.ConstrainedExecution.CriticalFinalizerObject> 类派生的类中执行关键终结器和使用 <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A> 方法执行的代码。  
  
## <a name="cer-advance-preparation"></a>CER 事先准备  

 CLR 事先准备 CER 以避免出现内存不足的情况。 需事先准备，防止 CLR 在实时编译或类型加载时发生内存不足的情况。  
  
 开发者需要指定一个代码区域作为 CER：  
  
- 事先已准备好顶级 CER 区域和完整调用关系图中应用了 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> 属性的方法。 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> 只能保证 <xref:System.Runtime.ConstrainedExecution.Cer.Success> 或 <xref:System.Runtime.ConstrainedExecution.Cer.MayFail> 状态。  
  
- 不能对无法静态确定的调用（如虚拟调度）执行事先准备。 请在这些情况中使用 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A> 方法。 使用 <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A> 方法时，应对清理代码应用 <xref:System.Runtime.ConstrainedExecution.PrePrepareMethodAttribute> 属性。  
  
## <a name="constraints"></a>约束  

 用户可写入 CER 的代码类型受限。 代码无法导致带外异常，可能是以下操作引起的：  
  
- 显式分配。  
  
- 装箱。  
  
- 获取锁。  
  
- 虚拟调用未准备好的方法。  
  
- 调用弱可靠性协定或不存在可靠性协定的方法。  
  
 在.NET Framework version 2.0 版中，这些约束称为准则。 通过代码分析工具提供诊断。  
  
## <a name="reliability-contracts"></a>可靠性协定  

 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> 是记录给定方法的可靠性保证和损坏状态的自定义属性。  
  
### <a name="reliability-guarantees"></a>可靠性保证  

 可靠性保证由 <xref:System.Runtime.ConstrainedExecution.Cer> 枚举值表示，指示给定方法的可靠程度：  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.MayFail>. 异常情况下，方法可能会失败。 在这种情况下，方法会向调用方法报告是成功还是失败。 方法必须包含在 CER 中，以确保它可以报告返回值。  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.None>. 方法、类型和程序集没有 CER 的概念，如果状态损坏没有得到实质性的缓解，在 CER 内进行调用很可能是不安全的。 它不利用 CER 保证。 这意味着：  
  
    1. 异常情况下，方法可能会失败。  
  
    2. 方法可能报告失败，也可能不报告失败。  
  
    3. 最可能的方案是，未将方法编写为使用 CER。  
  
    4. 如果方法、类型和程序集没有显示标识为成功，则会隐式标识为 <xref:System.Runtime.ConstrainedExecution.Cer.None>。  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.Success>. 异常情况下，保证方法一定成功。 若要达到这种级别的可靠性，应始终在调用的方法周围构造 CER，即使方法是从非 CER 区域调用的。 如果方法完成了预期任务，它就是成功的，虽然这种成功可能只是主观意义上的成功。 例如，用 `ReliabilityContractAttribute(Cer.Success)` 标记计数意味着当它在 CER 下运行时，它始终返回 <xref:System.Collections.ArrayList> 中的元素的数目计数，并且它永远不能将内部的字段保留为不确定状态。  但是，<xref:System.Threading.Interlocked.CompareExchange%2A> 方法也标记为成功，此处的成功意味着该值不会因争用条件而替换为新值。  关键在于方法的行为方式与记录的行为方式相同，不需要编写 CER 代码来预期正确但不可靠的代码行为之外的任何异常行为。  
  
### <a name="corruption-levels"></a>损坏级别  

 损坏级别由 <xref:System.Runtime.ConstrainedExecution.Consistency> 枚举值表示，指示给定坏境中状态的损坏程度：  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptAppDomain>. 异常情况下，公共语言运行时 (CLR) 对当前应用改程序域中的状态一致性不做任何保证。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptInstance>. 异常情况下，方法保证将状态损坏限制为当前实例。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptProcess>，异常情况下，CLR 对状态一致性不做任何保证；即这种情况可能会损坏进程。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.WillNotCorruptState>. 异常情况下，方法保证不损坏状态。  
  
## <a name="reliability-trycatchfinally"></a>可靠性 try/catch/finally  

 可靠性 `try/catch/finally` 是一种异常处理机制，其预知性保证的级别与非托管版本相同。 `catch/finally` 块为 CER。 块中的方法需要事先准备，并且必需是不可中断的。  
  
 在 .NET Framework 2.0 版中，代码通过在 try 块之前直接调用 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 来通知运行时 try 是可靠的。 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 是编译器支持类 <xref:System.Runtime.CompilerServices.RuntimeHelpers> 的成员。 通过使用编译器暂停可用性，直接调用 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A>。  
  
## <a name="noninterruptible-regions"></a>不可中断区域  

 不可中断区域将一组指令分组到 CER。  
  
 在 .NET Framework 2.0 版中，通过使用编译器支持暂停可用性，用户代码创建不可中断的区域，其中具有包含前面是 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 方法调用的空 try/catch 块的可靠 try/catch/finally。  
  
## <a name="critical-finalizer-object"></a>关键终结器对象  

 <xref:System.Runtime.ConstrainedExecution.CriticalFinalizerObject> 保证垃圾回收会执行终结器。 进行分配时，应事先准备好终结器及其调用关系图。 终结器方法在 CER 中执行，并且必须遵从所有关于 CER 和终结器的约束。  
  
 从 <xref:System.Runtime.InteropServices.SafeHandle> 和 <xref:System.Runtime.InteropServices.CriticalHandle> 继承的任何类型都保证在 CER 内执行其终结器。 在 <xref:System.Runtime.InteropServices.SafeHandle> 派生的类中实现 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 以执行释放图柄所需的任何代码。  
  
## <a name="code-not-permitted-in-cers"></a>CER 中不允许的代码  

 CER 中不允许以下操作：  
  
- 显示分配。  
  
- 获取锁。  
  
- 装箱。  
  
- 多维数组访问。  
  
- 通过反射进行的方法调用。  
  
- <xref:System.Threading.Monitor.Enter%2A> 或 <xref:System.IO.FileStream.Lock%2A>。  
  
- 安全检查。 不执行请求，仅链接请求。  
  
- COM 对象和代理的 <xref:System.Reflection.Emit.OpCodes.Isinst> 和 <xref:System.Reflection.Emit.OpCodes.Castclass>  
  
- 获取或设置透明代理上的字段。  
  
- 序列化。  
  
- 函数函数指针和委托。  
  
## <a name="see-also"></a>另请参阅

- [可靠性最佳做法](reliability-best-practices.md)
