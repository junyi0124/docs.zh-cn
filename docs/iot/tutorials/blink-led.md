---
title: 闪烁 LED
description: 了解如何使用 .NET IoT 库闪烁 LED。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: 07a050c0655f9cae3d7f2b924c0dd07ac1c6c0b9
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96591092"
---
# <a name="blink-an-led"></a>闪烁 LED

可以单独控制常规用途 i/o (GPIO) pin。 这适用于控制 Led、中继和其他有状态设备。 在本主题中，你将使用 .NET 和 Raspberry Pi 的 GPIO pin 来使 LED 通电，并重复闪烁。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- 5 mm LED
- 330Ω电阻器
- 试验板
- 跳线
- Raspberry Pi GPIO (可选/推荐的) 
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

[!INCLUDE [ensure-ssh](../includes/ensure-ssh.md)]

## <a name="prepare-the-hardware"></a>准备硬件

按照下图所示，使用硬件组件构建线路：

:::image type="content" source="../media/rpi-led_bb-thumb.png" alt-text="显示具有 LED 和电阻器的线路的 Fritzing 图" lightbox="../media/rpi-led_bb.png":::

上图描述了下列连接：

- GPIO 18 到 LED 阳极 (更长的正面) 
- LED (较短、负面的潜在顾客) 到330Ω电阻器 (要么结束) 
- 330Ω电阻器 (其他端) 到地面

[!INCLUDE [tutorial-rpi-gpio](../includes/tutorial-rpi-gpio.md)]

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md)创建一个新的 .net 控制台应用程序。 将其命名为 *BlinkTutorial*。

    ```dotnetcli
    dotnet new console -o BlinkTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/BlinkTutorial/Program.cs" :::

    在上述代码中：

    - [Using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)创建的实例 `GpioController` 。 `using`声明可确保释放对象并正确释放硬件资源。
    - 为输出打开 GPIO 引脚18
    - `while`循环会无限期运行。 每次迭代：
        1. 向 GPIO 引脚18写入值。 如果 `ledOn` 为 true，则它 `PinValue.High` 在) 上写入 (。 否则，将写入 `PinValue.Low` 。
        1. 睡眠1000毫秒。
        1. 切换的值 `ledOn` 。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./BlinkTutorial
    ```

    LED 闪烁，每秒闪烁一次。

1. 通过按 <kbd>Ctrl + C</kbd>终止节目。

恭喜！ 已使用 GPIO 闪烁 LED。

## <a name="get-the-source-code"></a>获取源代码

[GitHub 上提供](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/BlinkTutorial)了本教程的源 <span class="docon docon-navigate-external x-hidden-focus"></span> 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何从传感器读取环境条件](../tutorials/temp-sensor.md)
