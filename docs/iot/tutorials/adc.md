---
title: 从模拟到数字转换器读取值
description: 了解如何使用模拟到数字转换器读取可变电压值。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: 7cf25f181997ed66639842727be57e7824ef5466
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739982"
---
<!--markdownlint-disable DOCSMD011 -->
# <a name="read-values-from-an-analog-to-digital-converter"></a>从模拟到数字转换器读取值

 (ADC) 使用模拟到数字的转换器，它可以读取模拟输入电压值并将其转换为数字值。 ADCs 用于从 thermistors、potentiometers 和其他设备读取值，这些值根据某些条件更改抵抗。

在本主题中，你将使用 .NET 从 ADC 读取值，因为使用 potentiometer 伯输入电压。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [MCP3008](https://www.microchip.com/wwwproducts/MCP3008) <span class="docon docon-navigate-external x-hidden-focus"></span> 模拟到数字转换器
- 三针 potentiometer
- 试验板
- 跳线
- Raspberry Pi GPIO (可选/推荐的) 
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

[!INCLUDE [prepare-pi-spi](../includes/prepare-pi-spi.md)]

## <a name="prepare-the-hardware"></a>准备硬件

按照下图所示，使用硬件组件构建线路：

:::image type="content" source="../media/rpi-trimpot_spi-thumb.png" alt-text="显示具有 MCP3008 ADC 和 potentiometer 的线路的 Fritzing 关系图" lightbox="../media/rpi-trimpot_spi.png":::

MCP3008 使用串行外围设备接口 (SPI) 进行通信。 下面是从 MCP3008 到 Raspberry Pi 和 potentiometer 的连接：

- 从<sub>DD</sub> 到 3.3 v (显示为红色) 
- 指向 3.3 V 的 v<sub>引用</sub> (红色) 
- AGND 到地面 (黑色) 
- CLK SCLK (橙色) 
- D<sub>MISO</sub> (橙色) 
- D<sub>中的</sub> MOSI (橙色) 
- CS/SHDN to CE0 (绿色) 
- DGND 到地面 (黑色) 
- CH0 到 potentiometer (黄色的变量 (中间) 固定) 

向 potentiometer 上的外部针脚提供 3.3 V 和接地。 顺序并不重要。

根据需要，请参阅以下引出线关系图：

| MCP3008  | Raspberry Pi GPIO |
|----------|-------------------|
| :::image type="content" source="../media/mcp3008-diagram-thumb.png" alt-text="显示 MCP3008 引线的示意图" lightbox="../media/mcp3008-diagram.png"::: | :::image type="content" source="../media/gpio-pinout-diagram-thumb.png" alt-text="显示 Raspberry Pi GPIO 标头引线的关系图。Image 礼节性 Raspberry Pi Foundation。" lightbox="../media/gpio-pinout-diagram.png":::<br />[Image 礼节性 Raspberry Pi Foundation](https://www.raspberrypi.org/documentation/usage/gpio/)。
 |

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md)创建一个新的 .net 控制台应用程序。 将其命名为 *AdcTutorial*。

    ```dotnetcli
    dotnet new console -o AdcTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/AdcTutorial/Program.cs" :::

    在上述代码中：

    - `hardwareSpiSettings` 设置为的新实例 `SpiConnectionSettings` 。 构造函数将 `busId` 参数设置为0，将 `chipSelectLine` 参数设置为0。
    - [Using 声明](../../csharp/whats-new/csharp-8.md#using-declarations) `SpiDevice` 通过调用并传入来创建的实例 `SpiDevice.Create` `hardwareSpiSettings` 。 这 `SpiDevice` 表示 SPI 总线。 `using`声明可确保释放对象并正确释放硬件资源。
    - 其他 `using` 声明将创建的实例 `Mcp3008` ，并将传递 `SpiDevice` 到构造函数中。
    - `while`循环会无限期运行。 每次迭代：
        1. 通过调用读取 ADC 上 CH0 的值 `mcp.Read(0)` 。
        1. 值除以10.24。 MCP3008 是一个10位 ADC，这意味着它将返回1024的可能值，范围为0-1023。 将值除以10.24 表示百分比形式的值。
        1. 将值舍入到最接近的整数。
        1. 将值写入格式为百分比的控制台。
        1. 睡眠500毫秒。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./AdcTutorial
    ```

    旋转 potentiometer 时观察输出。 这是因为 potentiometer 会将在 ADC 上提供给 CH0 的电压改变。 ADC 比较了 CH0 上的输入电压与 V<sub>REF</sub> 提供的参考电压，以生成值。

1. 通过按 <kbd>Ctrl + C</kbd>终止节目。

恭喜！ 已使用 SPI 从模拟到数字转换器读取值。

## <a name="get-the-source-code"></a>获取源代码

[GitHub 上提供](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/AdcTutorial)了本教程的源 <span class="docon docon-navigate-external x-hidden-focus"></span> 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用常规用途输入/输出使 LED 闪烁](../tutorials/blink-led.md)
