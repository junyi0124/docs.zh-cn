---
title: 快速入门-使用 .NET 驱动 Raspberry Pi 的 HAT
description: 在5分钟内开始使用 .NET IoT 库，这是 Raspberry Pi 的附加板。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: quickstart
ms.prod: dotnet
ms.openlocfilehash: 2919db55304590f5557aa0cbda50cc4bd6640443
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739525"
---
# <a name="quickstart---use-net-to-drive-a-raspberry-pi-sense-hat"></a>快速入门-使用 .NET 驱动 Raspberry Pi 的 HAT

Raspberry Pi [HAT](https://www.raspberrypi.org/products/sense-hat/) <span class="docon docon-navigate-external x-hidden-focus"></span> (**H** 硬件 ttached **A** on **T** op) 是 Raspberry Pi 的附加板。 此感知 HAT 配有8× 8 RGB LED 矩阵、五按钮游戏杆，并包括以下传感器：

- 陀螺仪
- 加速计
- 磁力计
- 温度
- 大气压力
- 湿度

此快速入门使用 .NET 从感知 HAT 检索传感器值、响应游戏杆输入，并推动 LED 矩阵。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- 感知 HAT

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="run-the-quickstart"></a>运行快速入门

将含义 HAT 附加到 Raspberry Pi。 在 Raspberry Pi 上的 Bash 提示符 (本地或远程) 上，运行以下命令：

```bash
. <(wget -q -O - https://aka.ms/dotnet-iot-sensehat-quickstart)
```

命令下载并运行脚本。 脚本：

- 安装 .NET SDK。
- 克隆包含感知 HAT 快速入门项目的 GitHub 存储库。
- 生成项目。
- 运行该项目。

显示传感器数据时，请查看控制台输出。 LED 矩阵在蓝色字段上显示黄色像素。 在任意方向上握住操纵杆会在该方向上移动黄色像素。 单击中心游戏杆按钮会使背景从蓝色切换到红色。

## <a name="get-the-source-code"></a>获取源代码

此快速入门的源 [可在 GitHub 上找到](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/quickstarts/SenseHat.Quickstart) <span class="docon docon-navigate-external x-hidden-focus"></span> 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用常规用途输入/输出使 LED 闪烁](../tutorials/blink-led.md)
