---
title: releaseHandleFailed MDA
description: 查看 releaseHandleFailed 托管调试助手 (MDA) ，这可能是由于 .NET 中的资源或内存泄漏导致的。
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), handles
- release handle failed
- CriticalHandle class, run-time errors
- releaseHandleFailed MDA
- ReleaseHandle method
- SafeHandle class, run-time errors
- MDAs (managed debugging assistants), handles
ms.assetid: 44cd98ba-95e5-40a1-874d-e8e163612c51
ms.openlocfilehash: b337a7283e961d0fae2b51d92a21fa77f7249250
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267125"
---
# <a name="releasehandlefailed-mda"></a>releaseHandleFailed MDA

当 <xref:System.Runtime.InteropServices.SafeHandle> 或 <xref:System.Runtime.InteropServices.CriticalHandle> 派生的类的 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法返回 `false` 时，激活 `releaseHandleFailed` 托管调试助手 (MDA) 通知开发人员。  
  
## <a name="symptoms"></a>症状  

 资源或内存泄漏。  如果 <xref:System.Runtime.InteropServices.SafeHandle> 或 <xref:System.Runtime.InteropServices.CriticalHandle> 派生的类的 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法失败，则由类所封装的资源可能尚未释放或清理。  
  
## <a name="cause"></a>原因  

 如果用户创建派生自 <xref:System.Runtime.InteropServices.SafeHandle> 或 <xref:System.Runtime.InteropServices.CriticalHandle> 的类，则他们必须提供 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法的实现；因此，这些情况是特定于单个资源的。 但是，需求如下所示：  
  
- <xref:System.Runtime.InteropServices.SafeHandle> 和 <xref:System.Runtime.InteropServices.CriticalHandle> 类型表示重要进程资源的包装。 随着时间的推移，内存泄漏会使该过程不可用。  
  
- <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法必须成功执行其功能。 一旦进程获取此类资源，<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 就是唯一将其释放的方法。 因此，失败意味着资源泄漏。  
  
- 在执行 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 期间发生的任何故障阻碍了资源释放，这是实现 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法本身的 Bug。 即使该代码调用由其他人授权的代码来执行其功能，程序员也要负责确保履行该合同。  
  
## <a name="resolution"></a>解决方法  

 应检查使用引发 MDA 通知的特定 <xref:System.Runtime.InteropServices.SafeHandle>（或 <xref:System.Runtime.InteropServices.CriticalHandle>）类型的代码，查找原始句柄值从 <xref:System.Runtime.InteropServices.SafeHandle> 提取的位置或复制的其他位置。 这是 <xref:System.Runtime.InteropServices.SafeHandle> 或 <xref:System.Runtime.InteropServices.CriticalHandle> 实现失败的一般原因，由于原始句柄值的使用在那时将不再由运行时跟踪。 如果原始句柄复制随后关闭，则可能导致之后的 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 调用失败，因为关闭是在相同句柄上尝试的，而该句柄现已无效。  
  
 可能不正确的句柄复制，有多种方式：  
  
- 查找对 <xref:System.Runtime.InteropServices.SafeHandle.DangerousGetHandle%2A> 方法的调用。 对此方法的调用应极其罕见，以及对 <xref:System.Runtime.InteropServices.SafeHandle.DangerousAddRef%2A> 和 <xref:System.Runtime.InteropServices.SafeHandle.DangerousRelease%2A> 方法的调用应围绕你的任何发现。 后面的方法指定原始句柄值能安全使用的代码区域。 在此区域外，或如果引用计数永远不会增加至第一，则可以在任何时候通过调用另一线程上的 <xref:System.Runtime.InteropServices.SafeHandle.Dispose%2A> 或 <xref:System.Runtime.InteropServices.SafeHandle.Close%2A> 使句柄值失效。 一旦所有对 <xref:System.Runtime.InteropServices.SafeHandle.DangerousGetHandle%2A> 的使用都被跟踪，则应该跟随原始句柄采用的路径，确保它不会被移交给将最终调用 `CloseHandle` 的某些组件或将释放句柄的另一低级别的本机方法。  
  
- 确保用以初始化带有有效原始句柄值的 <xref:System.Runtime.InteropServices.SafeHandle> 的代码拥有句柄。 如果在没有将 `ownsHandle` 参数设置到基构造函数中的 `false` 时，围绕代码没有的句柄构造 <xref:System.Runtime.InteropServices.SafeHandle>，则 <xref:System.Runtime.InteropServices.SafeHandle> 和实际句柄拥有者都可以关闭该句柄，如果 <xref:System.Runtime.InteropServices.SafeHandle> 失去争用，这将会导致 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 中出错。  
  
- 当 <xref:System.Runtime.InteropServices.SafeHandle> 在应用程序域之间封送，请确认正在使用 <xref:System.Runtime.InteropServices.SafeHandle> 派生已被标记为可序列化。 在极少数情况下，当派生自 <xref:System.Runtime.InteropServices.SafeHandle> 的类已进行序列化，它应实现 <xref:System.Runtime.Serialization.ISerializable> 接口或使用其他方法之一来手动控制序列化和反序列化进程。 这是必需的，因为默认序列化操作是为了创建封闭原始句柄值的按位克隆，从而生成两个 <xref:System.Runtime.InteropServices.SafeHandle> 拥有相同句柄的实例。 两者都将尝试在某个时刻调用相同句柄上的 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A>。 执行此操作的第二个 <xref:System.Runtime.InteropServices.SafeHandle> 将失败。 序列化 <xref:System.Runtime.InteropServices.SafeHandle> 时的正确操作过程是为本机句柄类型调用 `DuplicateHandle` 功能或类似功能，以进行不同的合法句柄复制。 如果句柄类型不支持，则包装它的 <xref:System.Runtime.InteropServices.SafeHandle> 类型不能设置为可序列化。  
  
- 可以跟踪句柄之前关闭的位置，通过将调试器断点放在用来释放该句柄的本机例程上（例如 `CloseHandle` 函数），当最终调用 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法时，会导致失败。 这在压力方案中或甚至是中型功能测试中都不可能实现，由于此类例程处理的通信通常非常繁忙。 它可能有助于检测调用本机释放方法的代码，以便捕获调用方的标识，或者可能捕获完整堆栈跟踪，以及被释放句柄的值。  句柄值可以与此 MDA 报告的值进行比较。  
  
- 注意：某些本机句柄类型（例如所有可以通过 `CloseHandle` 函数释放的 Win32 处理程序）会共享相同的句柄命名空间。 一个句柄类型的错误释放会导致另一个句柄类型出现问题。 例如，意外地两次关闭 Win32 事件句柄可能会导致过早关闭一个明显不相关的文件句柄。 当释放该句柄和句柄值可使用于跟踪另一个资源（可能为另一种类型）时，将发生这种情况。 如果发生这种情况，并且后跟错误的第二次释放，则不相关线程句柄可能会失效。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 一条消息，该消息表明 <xref:System.Runtime.InteropServices.SafeHandle> 或 <xref:System.Runtime.InteropServices.CriticalHandle> 未能正确释放该句柄。 例如：  
  
```output
"A SafeHandle or CriticalHandle of type 'MyBrokenSafeHandle'
failed to properly release the handle with value 0x0000BEEF. This
usually indicates that the handle was released incorrectly via
another means (such as extracting the handle using DangerousGetHandle
and closing it directly or building another SafeHandle around it."  
```  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <releaseHandleFailed/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下是可激活 `releaseHandleFailed` MDA 的代码示例。  
  
```csharp
bool ReleaseHandle()  
{  
    // Calling the Win32 CloseHandle function to release the
    // native handle wrapped by this SafeHandle. This method returns
    // false on failure, but should only fail if the input is invalid
    // (which should not happen here). The method specifically must not
    // fail simply because of lack of resources or other transient
    // failures beyond the user’s control. That would make it unacceptable
    // to call CloseHandle as part of the implementation of this method.  
    return CloseHandle(handle);  
}  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
