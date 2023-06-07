---
title: 在 LCD 上显示文本
description: 了解如何使用 .NET IoT 库在液晶显示器上显示字符。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: d4c3e373207e23877903491871f4d09e11000c1a
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96591087"
---
<!--markdownlint-disable DOCSMD011 -->
# <a name="display-text-on-an-lcd"></a>在 LCD 上显示文本

LCD 字符显示适用于显示信息，而无需使用外部监视器。 常见 LCD 字符显示可以直接连接到 GPIO pin，但这种方法要求使用最多10个 GPIO pin。 对于需要连接到设备组合的方案，将花费的许多 GPIO 标头的操作通常是不切实际的。

许多制造商通过集成的 GPIO 扩展器销售 20x4 LCD 字符显示。 字符显示直接连接到 GPIO 扩展器，然后通过 Inter-Integrated 线路 (I2C) 串行协议连接到 Raspberry Pi。

在本主题中，你将使用 "使用 I2C GPIO" 扩展器通过 .NET 显示 LCD 字符显示的文本。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [带有 I2C 接口的 20X4 LCD 字符显示](https://www.bing.com/images/search?q=20x4+lcd+display+with+i2c)<span class="docon docon-navigate-external x-hidden-focus"></span>
- 跳线
- 试验板 (可选/推荐) 
- Raspberry Pi GPIO (可选/推荐的) 
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

> [!NOTE]
> LCD 字符显示有许多制造商。 大多数设计都是相同的，并且制造商不会对功能产生任何影响。 作为参考，本教程是通过 [SUNFOUNDER LCD2004](https://www.sunfounder.com/lcd2004-module.html)开发的 <span class="docon docon-navigate-external x-hidden-focus"></span> 。

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="prepare-the-hardware"></a>准备硬件

使用跳线线路将 I2C GPIO 扩展器上的四个 pin 连接到 Raspberry Pi，如下所示：

- GND 到地面
- 与5V 的 VCC
- SDA to SDA (GPIO 2) 
- 在 GPIO 3) SCL 到 SCL (

根据需要，请参阅以下数字：

| I2C 接口 (显示的背面)  | Raspberry Pi GPIO |
|---------------------------------|-------------------|
| :::image type="content" source="../media/character-display-i2c-thumb.png" alt-text="显示 I2C GPIO 扩展器的字符显示的背景图像。" lightbox="../media/character-display-i2c.png"::: | :::image type="content" source="../media/gpio-pinout-diagram-thumb.png" alt-text="显示 Raspberry Pi GPIO 标头引线的关系图。Image 礼节性 Raspberry Pi Foundation。" lightbox="../media/gpio-pinout-diagram.png":::<br />[Image 礼节性 Raspberry Pi Foundation](https://www.raspberrypi.org/documentation/usage/gpio/) <span class="docon docon-navigate-external x-hidden-focus"></span> 。
 |

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md)创建一个新的 .net 控制台应用程序。 将其命名为 *LcdTutorial*。

    ```dotnetcli
    dotnet new console -o LcdTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/LcdTutorial/Program.cs" :::

    在上述代码中：

    - [使用声明](../../csharp/whats-new/csharp-8.md#using-declarations) `I2cDevice` 通过调用 `I2cDevice.Create` 并传入 `I2cConnectionSettings` 具有和参数的新来 `busId` 创建的实例 `deviceAddress` 。 这 `I2cDevice` 表示 I2C 总线。 `using`声明可确保释放对象并正确释放硬件资源。

        > [!WARNING]
        > GPIO 扩展器的设备地址取决于制造商使用的芯片。 使用 PCF8574A 芯片的 GPIO 扩展器提供了 PCF8574 `0x27` `0x3F` 。 请查阅您的 LCD 文档。

    - 其他 `using` 声明将创建的实例 `Pcf8574` ，并将传递 `I2cDevice` 到构造函数中。 此实例表示 GPIO 扩展器。
    - 另一个 `using` 声明创建了一个 `Lcd2004` 用于表示显示的实例。 向构造函数传递几个参数，描述用于与 GPIO 扩展器进行通信的设置。 GPIO 扩展器作为 `controller` 参数传递。
    - `while`循环会无限期运行。 每次迭代：
        1. 清除显示。
        1. 将光标位置设置为当前行上的第一个位置。
        1. 将当前时间写入当前光标位置的显示位置。
        1. 循环访问当前行计数器。
        1. 睡眠1000毫秒。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./LcdTutorial
    ```

    观察 LCD 字符显示，因为当前时间显示在每一行上。

    > [!TIP]
    > 如果显示屏已亮起但看不到任何文本，请尝试调整显示屏背面的对比度。

1. 通过按 <kbd>Ctrl + C</kbd>终止节目。

恭喜！ 已使用 I2C 和 GPIO 扩展器在液晶屏上显示文本！

## <a name="get-the-source-code"></a>获取源代码

[GitHub 上提供](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/LcdTutorial)了本教程的源 <span class="docon docon-navigate-external x-hidden-focus"></span> 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用常规用途输入/输出使 LED 闪烁](../tutorials/blink-led.md)
