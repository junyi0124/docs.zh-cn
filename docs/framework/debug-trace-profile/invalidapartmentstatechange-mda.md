---
title: invalidApartmentStateChange MDA
description: 了解 .NET 中的 invalidApartmentStateChange 托管调试助手 (MDA) ，如果 COM 单元状态有问题，则会激活该助手。
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), invalid apartment state
- managed debugging assistants (MDAs), invalid apartment state
- InvalidApartmentStateChange MDA
- invalid apartment state changes
- threading [.NET Framework], apartment states
- apartment states
- threading [.NET Framework], managed debugging assistants
- COM apartment states
ms.assetid: e56fb9df-5286-4be7-b313-540c4d876cd7
ms.openlocfilehash: db55e3ac2d6862d008013abef0f09f67213d9faa
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272743"
---
# <a name="invalidapartmentstatechange-mda"></a>invalidApartmentStateChange MDA

`invalidApartmentStateChange` 托管调试助手 (MDS) 通过以下两种问题中的任何一种激活：  
  
- 尝试将已由 COM 初始化的线程的 COM 单元状态更改为不同的单元状态。  
  
- 线程的 COM 单元状态意外更改。  
  
## <a name="symptoms"></a>症状  
  
- 线程的 COM 单元状态不符合请求。 这可能造成用于已有线程模型的 COM 组件的代理不同于现有代理。 进而可能导致在通过未设置为跨单元封送的接口调用 COM 对象时，引发 <xref:System.InvalidCastException>。  
  
- 线程的 COM 单元状态不同于预期。 这可能造成调用[运行时可调用包装器](../../standard/native-interop/runtime-callable-wrapper.md) (RCW) 时，出现 <xref:System.Runtime.InteropServices.COMException>、RPC_E_WRONG_THREAD 返回 HRESULT 以及 <xref:System.InvalidCastException>。 这也可能造成部分单线程 COM 组件由多个线程同时访问，进而导致损坏或数据丢失。  
  
## <a name="cause"></a>原因  
  
- 该线程之前已初始化为不同的 COM 单元状态。 请注意，可显式或隐式设置线程的单元状态。 显式操作包括 <xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType> 属性以及 <xref:System.Threading.Thread.SetApartmentState%2A> 和 <xref:System.Threading.Thread.TrySetApartmentState%2A> 方法。 通过 <xref:System.Threading.Thread.Start%2A> 方法创建的线程可隐式设置为 <xref:System.Threading.ApartmentState.MTA>，除非 <xref:System.Threading.Thread.SetApartmentState%2A> 在线程创建之前就已调用。 应用程序主线程同样可隐式初始化为 <xref:System.Threading.ApartmentState.MTA>，除非在主方法上指定了 <xref:System.STAThreadAttribute> 属性。  
  
- 在线程上调用具有不同并发模型的 `CoUninitialize` 方法（或 `CoInitializeEx` 方法）。  
  
## <a name="resolution"></a>解决方法  

 在单元状态开始执行前对其进行设置，或将 <xref:System.STAThreadAttribute> 属性或 <xref:System.MTAThreadAttribute> 属性应用到应用程序的主方法。  
  
 对于第二种结果，理想情况下，需要将调用 `CoUninitialize` 方法的代码修改为延迟调用，直到线程即将终止并且该线程停止使用 RCW 及其基础 COM 组件。 但是，如果无法修改调用 `CoUninitialize` 方法的代码，则未采用此方法进行初始化的线程不能使用任何 RCW。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 当前线程的 COM 单元状态和试图应用的代码状态。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidApartmentStateChange />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下代码示例展示了一种可激活该 MDA 的情况。  
  
```csharp
using System.Threading;  
namespace ApartmentStateMDA  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            Thread.CurrentThread.SetApartmentState(ApartmentState.STA);  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
