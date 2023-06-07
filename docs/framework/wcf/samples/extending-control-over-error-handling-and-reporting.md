---
title: 扩展对错误处理和错误报告的控制
ms.date: 03/30/2017
ms.assetid: 45f996a7-fa00-45cb-9d6f-b368f5778aaa
ms.openlocfilehash: 2e9298c6a282b9df8499458ad166e320d41e63a9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283323"
---
# <a name="extending-control-over-error-handling-and-reporting"></a>扩展对错误处理和错误报告的控制

此示例演示如何使用接口在 Windows Communication Foundation (WCF) 服务中扩展对错误处理和错误报告的控制 <xref:System.ServiceModel.Dispatcher.IErrorHandler> 。 该示例基于 [入门](getting-started-sample.md) ，并向服务添加了一些附加代码来处理错误。 客户端强制实施若干个错误条件。 服务将截获这些错误并将其记录到某个文件中。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 服务可以截获错误、执行处理，并使用 <xref:System.ServiceModel.Dispatcher.IErrorHandler> 接口影响错误报告的方式。 该接口有两个可以实现的方法：<xref:System.ServiceModel.Dispatcher.IErrorHandler.ProvideFault%28System.Exception%2CSystem.ServiceModel.Channels.MessageVersion%2CSystem.ServiceModel.Channels.Message%40%29> 和 <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A>。 通过 <xref:System.ServiceModel.Dispatcher.IErrorHandler.ProvideFault%28System.Exception%2CSystem.ServiceModel.Channels.MessageVersion%2CSystem.ServiceModel.Channels.Message%40%29> 方法，您可以添加、修改或取消为响应异常而生成的错误消息。 通过 <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> 方法，您可以在发生错误时对错误进行处理，并控制是否可以运行其他错误处理。  
  
 在此示例中，`CalculatorErrorHandler` 类型实现 <xref:System.ServiceModel.Dispatcher.IErrorHandler> 接口。 在  
  
 <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> 方法中，`CalculatorErrorHandler` 将错误日志写入 c:\logs 中的 Error.txt 文本文件中。 请注意，该示例记录错误而不会取消错误，并允许错误报告回客户端。  
  
```csharp
public class CalculatorErrorHandler : IErrorHandler
{
    // Provide a fault. The Message fault parameter can be replaced, or set to
    // null to suppress reporting a fault.

    public void ProvideFault(Exception error, MessageVersion version, ref Message fault)
    {
    }

    // HandleError. Log an error, then allow the error to be handled as usual.
    // Return true if the error is considered as already handled

    public bool HandleError(Exception error)
    {
        using (TextWriter tw = File.AppendText(@"c:\logs\error.txt"))
        {
            if (error != null)
            {
                tw.WriteLine("Exception: " + error.GetType().Name + " - " + error.Message);
            }
            tw.Close();
        }
        return true;
    }
}  
```  
  
 `ErrorBehaviorAttribute` 作为一种向服务注册错误处理程序的机制存在。 此属性采取单一类型的参数。 该类型应实现 <xref:System.ServiceModel.Dispatcher.IErrorHandler> 接口，还应具有一个公用的空构造函数。 该属性随后实例化该错误处理程序类型的实例，并将其安装到该服务中。 实现此操作的方法是，实现 <xref:System.ServiceModel.Description.IServiceBehavior> 接口，然后使用 <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> 方法将错误处理程序的实例添加到该服务。  
  
```csharp
// This attribute can be used to install a custom error handler for a service.  
public class ErrorBehaviorAttribute : Attribute, IServiceBehavior  
{  
    Type errorHandlerType;  
  
    public ErrorBehaviorAttribute(Type errorHandlerType)  
    {  
        this.errorHandlerType = errorHandlerType;  
    }  
  
    void IServiceBehavior.Validate(ServiceDescription description, ServiceHostBase serviceHostBase)  
    {  
    }  
  
    void IServiceBehavior.AddBindingParameters(ServiceDescription description, ServiceHostBase serviceHostBase, System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints, BindingParameterCollection parameters)  
    {  
    }  
  
    void IServiceBehavior.ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
    {  
        IErrorHandler errorHandler;  
  
        try  
        {  
            errorHandler = (IErrorHandler)Activator.CreateInstance(errorHandlerType);  
        }  
        catch (MissingMethodException e)  
        {  
            throw new ArgumentException("The errorHandlerType specified in the ErrorBehaviorAttribute constructor must have a public empty constructor.", e);  
        }  
        catch (InvalidCastException e)  
        {  
            throw new ArgumentException("The errorHandlerType specified in the ErrorBehaviorAttribute constructor must implement System.ServiceModel.Dispatcher.IErrorHandler.", e);  
        }  
  
        foreach (ChannelDispatcherBase channelDispatcherBase in serviceHostBase.ChannelDispatchers)  
        {  
            ChannelDispatcher channelDispatcher = channelDispatcherBase as ChannelDispatcher;  
            channelDispatcher.ErrorHandlers.Add(errorHandler);  
        }
    }  
}  
```  
  
 该示例实现计算器服务。 客户端通过提供具有非法值的参数，使该服务故意出现两个错误。 `CalculatorErrorHandler` 使用 <xref:System.ServiceModel.Dispatcher.IErrorHandler> 接口将这些错误记录到某个本地文件，然后允许将这些错误报告回客户端。 客户端强制执行除以零的除法运算和自变量超出范围的情况。  
  
```csharp
try
{  
    Console.WriteLine("Forcing an error in Divide");  
    // Call the Divide service operation - trigger a divide by 0 error.  
    value1 = 22;  
    value2 = 0;  
    result = proxy.Divide(value1, value2);  
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
}  
catch (FaultException e)  
{  
    Console.WriteLine("FaultException: " + e.GetType().Name + " - " + e.Message);  
}  
catch (Exception e)  
{  
    Console.WriteLine("Exception: " + e.GetType().Name + " - " + e.Message);  
}  
```  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 你将看到除以零的除法运算和自变量超出范围的情况被报告为错误。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Add(15,3) = 18  
Subtract(145,76) = 69  
Multiply(9,81) = 729  
Forcing an error in Divide  
FaultException: FaultException - Invalid Argument: The second argument must not be zero.  
Forcing an error in Factorial  
FaultException: FaultException - Invalid Argument: The argument must be greater than zero.  
  
Press <ENTER> to terminate client.  
```  
  
 文件 c:\logs\errors.txt 包含服务记录的有关错误的信息。 请注意，为了使服务能够写入目录，必须确保运行服务的进程 (通常 ASP.NET 或网络服务) 有权写入该目录。  
  
```txt
Fault: Reason = Invalid Argument: The second argument must not be zero.  
Fault: Reason = Invalid Argument: The argument must be greater than zero.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 确保您已经为 error.txt 文件创建了 c:\logs 目录， 或者修改 `CalculatorErrorHandler.HandleError` 中使用的文件名。  
  
4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\ErrorHandling`  
