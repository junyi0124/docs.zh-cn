---
title: 教程：使用 Windows Communication Foundation 客户端
description: 了解如何创建客户端实例、编译应用程序以及与服务进行通信，这是有关创建 WCF 应用程序的一系列文章。
ms.date: 03/19/2019
helpviewer_keywords:
- WCF clients [WCF], using
dev_langs:
- CSharp
- VB
ms.assetid: 190349fc-0573-49c7-bb85-8e316df7f31f
ms.openlocfilehash: 5c94d5f8af679580c4194aaaadeda759098953d2
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244330"
---
# <a name="tutorial-use-a-windows-communication-foundation-client"></a>教程：使用 Windows Communication Foundation 客户端

本教程介绍创建基本 Windows Communication Foundation （WCF）应用程序所需的五个任务中的最后一个。 有关教程的概述，请参阅[教程： Windows Communication Foundation 应用程序入门](getting-started-tutorial.md)。

创建并配置 Windows Communication Foundation （WCF）代理后，您可以创建一个客户端实例并编译该客户端应用程序。 然后，你可以使用它来与 WCF 服务进行通信。

在本教程中，你将了解如何执行以下操作：
> [!div class="checklist"]
>
> - 添加代码以使用 WCF 客户端。
> - 测试 WCF 客户端。

## <a name="add-code-to-use-the-wcf-client"></a>添加代码以使用 WCF 客户端

客户端代码执行以下步骤：

- 实例化 WCF 客户端。
- 从生成的代理调用服务操作。
- 完成操作调用后关闭客户端。

从**GettingStartedClient**项目中打开**Program.cs**或**Module1**文件，并将其代码替换为以下代码：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using GettingStartedClient.ServiceReference1;

namespace GettingStartedClient
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step 1: Create an instance of the WCF proxy.
            CalculatorClient client = new CalculatorClient();

            // Step 2: Call the service operations.
            // Call the Add service operation.
            double value1 = 100.00D;
            double value2 = 15.99D;
            double result = client.Add(value1, value2);
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

            // Call the Subtract service operation.
            value1 = 145.00D;
            value2 = 76.54D;
            result = client.Subtract(value1, value2);
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

            // Call the Multiply service operation.
            value1 = 9.00D;
            value2 = 81.25D;
            result = client.Multiply(value1, value2);
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

            // Call the Divide service operation.
            value1 = 22.00D;
            value2 = 7.00D;
            result = client.Divide(value1, value2);
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);

            // Step 3: Close the client to gracefully close the connection and clean up resources.
            Console.WriteLine("\nPress <Enter> to terminate the client.");
            Console.ReadLine();
            client.Close();
        }
    }
}
```

```vb
Imports System.Collections.Generic
Imports System.Text
Imports System.ServiceModel
Imports GettingStartedClient.ServiceReference1

Module Module1

    Sub Main()
        ' Step 1: Create an instance of the WCF proxy.
        Dim Client As New CalculatorClient()

        ' Step 2: Call the service operations.
        ' Call the Add service operation.
        Dim value1 As Double = 100D
        Dim value2 As Double = 15.99D
        Dim result As Double = Client.Add(value1, value2)
        Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)

        ' Call the Subtract service operation.
        value1 = 145D
        value2 = 76.54D
        result = Client.Subtract(value1, value2)
        Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result)

        ' Call the Multiply service operation.
        value1 = 9D
        value2 = 81.25D
        result = Client.Multiply(value1, value2)
        Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result)

        ' Call the Divide service operation.
        value1 = 22D
        value2 = 7D
        result = Client.Divide(value1, value2)
        Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result)

        ' Step 3: Close the client to gracefully close the connection and clean up resources.
        Console.WriteLine()
        Console.WriteLine("Press <Enter> to terminate the client.")
        Console.ReadLine()
        Client.Close()

    End Sub

End Module
```

请注意 `using` 导入的（对于 Visual c #）或 `Imports` （对于 Visual Basic）语句 `GettingStartedClient.ServiceReference1` 。 此语句将导入 Visual Studio 生成的带有**添加服务引用**函数的代码。 此代码实例化 WCF 代理，并调用计算器服务公开的每个服务操作。 然后，它会关闭代理并结束程序。

## <a name="test-the-wcf-client"></a>测试 WCF 客户端

### <a name="test-the-application-from-visual-studio"></a>从 Visual Studio 测试应用程序

1. 保存并生成解决方案。

2. 选择**GettingStartedLib**文件夹，然后从快捷菜单中选择 "**设为启动项目**"。

3. 从 "**启动项目**" 中，从下拉列表中选择 " **GettingStartedLib** "，然后选择 "**运行**" 或按**F5**。

### <a name="test-the-application-from-a-command-prompt"></a>在命令提示符下测试应用程序

1. 以管理员身份打开命令提示符，然后导航到你的 Visual Studio 解决方案目录。

2. 若要启动该服务：输入*GettingStartedHost\bin\Debug\GettingStartedHost.exe*。

3. 若要启动客户端：请打开另一个命令提示符，导航到你的 Visual Studio 解决方案目录，然后输入*GettingStartedClient\bin\Debug\GettingStartedClient.exe*。

   *GettingStartedHost.exe*生成以下输出：

   ```text
   The service is ready.
   Press <Enter> to terminate the service.

   Received Add(100,15.99)
   Return: 115.99
   Received Subtract(145,76.54)
   Return: 68.46
   Received Multiply(9,81.25)
   Return: 731.25
   Received Divide(22,7)
   Return: 3.14285714285714
   ```

   *GettingStartedClient.exe*生成以下输出：

   ```text
   Add(100,15.99) = 115.99
   Subtract(145,76.54) = 68.46
   Multiply(9,81.25) = 731.25
   Divide(22,7) = 3.14285714285714

   Press <Enter> to terminate the client.
   ```

## <a name="next-steps"></a>后续步骤

你现在已经完成了 WCF 入门教程中的所有任务。 在本教程中，你将了解：

在本教程中，你将了解如何执行以下操作：
> [!div class="checklist"]
>
> - 添加代码以使用 WCF 客户端。
> - 测试 WCF 客户端。

如果在任何步骤中遇到问题或错误，请按照故障排除一文中的步骤进行修复。

> [!div class="nextstepaction"]
> [排查 WCF 入门教程](troubleshooting-the-getting-started-tutorial.md)
