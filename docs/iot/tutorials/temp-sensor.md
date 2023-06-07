---
title: 从传感器读取环境条件
description: 了解如何通过 .NET IoT 库读取 termperature、大气压力和湿度。
author: camsoper
ms.author: casoper
ms.date: 11/14/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: 1270e7629e9afc12b1d76d260d4b8b51428f1040
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96591093"
---
# <a name="read-environmental-conditions-from-a-sensor"></a>从传感器读取环境条件

IoT 设备的最常见方案之一就是检测环境情况。 有多种传感器可用于监视温度、湿度、大气压力等。

在本主题中，你将使用 .NET 从传感器读取环境条件。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [BME280](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout) <span class="docon docon-navigate-external x-hidden-focus"></span> 湿度/大气压力/温度传感器分类
- 跳线
- 试验板 (可选) 
- Raspberry Pi GPIO (可选) 
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

> [!IMPORTANT]
> 有很多 BME280 取得突破的制造商。 大多数设计都是类似的，并且制造商不应对功能产生任何影响。 本教程尝试考虑变体。 确保 BME280 的 Inter-Integrated 线路 (I2C) 接口。

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="prepare-the-hardware"></a>准备硬件

按照下图所示，使用硬件组件构建线路：

:::image type="content" source="../media/rpi-bmp280_i2c-thumb.png" alt-text="显示从 Raspberry Pi 到 BME280 出局的连接的 Fritzing 关系图" lightbox="../media/rpi-bmp280_i2c.png":::

下面是从 Raspberry Pi 到 BME280 的连接：

- 3.3 v 到 VIN *或* 3V3 (显示为红色) 
- 与 GND 的地面 (黑色) 
- SDA (GPIO 2) 到 SDI *或* SDA (blue) 
- SCL (GPIO 3) 到 SCK *或* scl (橙色) 

[!INCLUDE [tutorial-rpi-gpio](../includes/tutorial-rpi-gpio.md)]

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md)创建一个新的 .net 控制台应用程序。 将其命名为 *SensorTutorial*。

    ```dotnetcli
    dotnet new console -o SensorTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/SensorTutorial/Program.cs" :::

    在上述代码中：

    - `i2cSettings` 设置为的新实例 `I2cConnectionSettings` 。 构造函数将 `busId` 参数设置为1，并将 `deviceAddress` 参数设置为 `Bme280.DefaultI2cAddress` 。

        > [!IMPORTANT]
        > 某些 BME280 的专题讨论会使用辅助地址值。 对于这些设备，请使用 `Bme280.SecondaryI2cAddress` 。

    - [Using 声明](../../csharp/whats-new/csharp-8.md#using-declarations) `I2cDevice` 通过调用并传入来创建的实例 `I2cDevice.Create` `i2cSettings` 。 这 `I2cDevice` 表示 I2C 总线。 `using`声明可确保释放对象并正确释放硬件资源。
    - 另一个 `using` 声明创建 `Bme280` 用于表示传感器的的实例。 在 `I2cDevice` 构造函数中传递。
    - 芯片使用芯片的当前 (默认值进行度量所需的时间) 设置通过调用来检索 `GetMeasurementDuration` 。
    - `while`循环会无限期运行。 每次迭代：
        1. 清除控制台。
        1. 将电源模式设置为 `Bmx280PowerMode.Forced` 。 这会强制芯片执行一个测量，存储结果，然后进入睡眠状态。
        1. 读取温度、压力、湿度和海拔量的值。

            > [!NOTE]
            > 海拔由设备绑定计算。 此重载 `TryReadAltitude` 使用平均海平面级压力来生成估算。

        1. 将当前环境条件写入控制台。
        1. 睡眠1000毫秒。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./SensorTutorial
    ```

    在控制台中查看传感器输出。

1. 通过按 <kbd>Ctrl + C</kbd>终止节目。

恭喜！ 你已使用 I2C 读取温度/湿度/大气压力传感器的值！

## <a name="get-the-source-code"></a>获取源代码

[GitHub 上提供](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/SensorTutorial)了本教程的源 <span class="docon docon-navigate-external x-hidden-focus"></span> 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何在液晶屏上显示文本](../tutorials/lcd-display.md)
