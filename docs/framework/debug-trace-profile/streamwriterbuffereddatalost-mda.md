---
title: streamWriterBufferedDataLost MDA
description: 查看 streamWriterBufferedDataLost 托管调试助手 (MDA) ，如果 StreamWriter 不将最后1到 4 KB 的数据写入文件，这可能会激活。
ms.date: 03/30/2017
helpviewer_keywords:
- StreamWriter class, data buffering problems
- managed debugging assistants (MDAs), StreamWriter data buffering
- buffers, StreamWriter problems
- MDAs (managed debugging assistants), StreamWriter data buffering
- StreamWriter buffered data lost
- data buffering problems
- streamWriterBufferedDataLost MDA
ms.assetid: 6e5c07be-bc5b-437a-8398-8779e23126ab
ms.openlocfilehash: 23a8146bfa5acc08000e689917abb844c5540fec
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267073"
---
# <a name="streamwriterbuffereddatalost-mda"></a>streamWriterBufferedDataLost MDA

写入 <xref:System.IO.StreamWriter> 时，将激活 `streamWriterBufferedDataLost` 托管调试助手 (MDA)，但随后，在销毁 <xref:System.IO.StreamWriter> 的实例前不再调用 <xref:System.IO.StreamWriter.Flush%2A> 或 <xref:System.IO.StreamWriter.Close%2A> 方法。 启用此 MDA 时，运行时确定 <xref:System.IO.StreamWriter> 内是否仍然有任何缓冲数据。 如果缓冲数据确实存在，则将激活 MDA。 调用 <xref:System.GC.Collect%2A> 和 <xref:System.GC.WaitForPendingFinalizers%2A> 方法可以强制运行终结器。 否则，终结器将似乎在任意时刻运行，并且在进程退出时可能根本没有运行。 在启用了此 MDA 的情况下显式运行终结器将有助于更可靠地重现此类问题。  
  
## <a name="symptoms"></a>症状  

 <xref:System.IO.StreamWriter> 不会将最后 1-4 KB 的数据写入文件。  
  
## <a name="cause"></a>原因  

 <xref:System.IO.StreamWriter> 内部缓冲数据，这要求调用 <xref:System.IO.StreamWriter.Close%2A> 或 <xref:System.IO.StreamWriter.Flush%2A> 方法将缓冲数据写入基础数据存储。 如果没有恰当调用 <xref:System.IO.StreamWriter.Close%2A> 或 <xref:System.IO.StreamWriter.Flush%2A>，则可能未按预期方式写入 <xref:System.IO.StreamWriter> 实例中缓冲的数据。  
  
 以下示例为此 MDA 应捕获的编写欠佳的代码。  
  
```csharp  
// Poorly written code.  
void Write()
{  
    StreamWriter sw = new StreamWriter("file.txt");  
    sw.WriteLine("Data");  
    // Problem: forgot to close the StreamWriter.  
}  
```  
  
 如果触发垃圾回收，则上述代码将更可靠地激活此 MDA，然后暂停，直到终结器完成操作。 要追踪此类问题，可在调试版本中将以下代码添加到上述方法的末尾。 这将有助于稳定地激活 MDA，但当然，这不能解决问题的根源。  
  
```csharp
GC.Collect();  
GC.WaitForPendingFinalizers();  
```  
  
## <a name="resolution"></a>解决方法  

 在关闭应用程序或具有 <xref:System.IO.StreamWriter> 实例的代码块之前，请确保在 <xref:System.IO.StreamWriter> 上调用 <xref:System.IO.StreamWriter.Close%2A> 或 <xref:System.IO.StreamWriter.Flush%2A>。 实现此操作的最佳机制之一是使用 C# `using` 块（Visual Basic 中为 `Using`）创建实例，这将确保调用编写器的 <xref:System.IO.StreamWriter.Dispose%2A> 方法，从而正确关闭实例。  
  
```csharp
using(StreamWriter sw = new StreamWriter("file.txt"))
{  
    sw.WriteLine("Data");  
}  
```  
  
 以下代码显示使用 `try/finally`而非 `using`）的相同解决方案。  
  
```csharp
StreamWriter sw;  
try
{  
    sw = new StreamWriter("file.txt"));  
    sw.WriteLine("Data");  
}  
finally
{  
    if (sw != null)  
        sw.Close();  
}  
```  
  
 如果这两个解决方案都不能使用（例如，如果 <xref:System.IO.StreamWriter> 存储在静态变量中，并且无法在其生命周期结束时轻松运行代码），则通过以下方式避免此问题：在最后一次使用 <xref:System.IO.StreamWriter> 上的 <xref:System.IO.StreamWriter.Flush%2A> 之后调用它，或者将 <xref:System.IO.StreamWriter.AutoFlush%2A> 属性设置为 `true`。  
  
```csharp
private static StreamWriter log;  
// static class constructor.  
static WriteToFile()
{  
    StreamWriter sw = new StreamWriter("log.txt");  
    sw.AutoFlush = true;  
  
    // Publish the StreamWriter for other threads.  
    log = sw;  
}  
```  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对运行时无任何影响。  
  
## <a name="output"></a>输出  

 指示发生了此冲突的消息。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <streamWriterBufferedDataLost />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.StreamWriter>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
