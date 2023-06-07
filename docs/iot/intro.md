---
title: 利用 .NET IoT 库为 IoT 设备开发应用
description: 了解如何使用 .NET 为 IoT 设备和方案生成应用程序。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: overview
ms.prod: dotnet
ms.openlocfilehash: c3d05ec5b05780f91404c3c27e91bcd602b0faeb
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "96591052"
---
# <a name="develop-apps-for-iot-devices-with-the-net-iot-libraries"></a>利用 .NET IoT 库为 IoT 设备开发应用

.NET 在各种平台和体系结构上运行。 支持常见物联网 (IoT) 板，如 Raspberry Pi 和 Hummingboard）。 IoT apps 通常与专用硬件（如传感器、模拟到数字转换器和 LCD 设备）交互。 .NET IoT 库可实现这些方案。

## <a name="video-overview"></a>视频概述

<!--markdownlint-disable MD034 -->
> [!VIDEO https://channel9.msdn.com/Series/IoT-101/Intro-to-IOT-with-NET-Core-1-of-9/player]

## <a name="libraries"></a>库

.NET IoT 库由两个 NuGet 包组成：

- [System.Device.Gpio](https://www.nuget.org/packages/System.Device.Gpio/) <span class="docon docon-navigate-external x-hidden-focus"></span>
- [Iot.Device.Bindings](https://www.nuget.org/packages/Iot.Device.Bindings/) <span class="docon docon-navigate-external x-hidden-focus"></span>

### <a name="systemdevicegpio"></a>System.Device.Gpio

`System.Device.Gpio` 支持各种协议，用于与低级别硬件 pin 交互以控制设备。 其中包括：

- 常规用途 I/O (GPIO)
- Inter-Integrated 线路 (I2C) 
- 串行外围设备接口 (SPI) 
- 脉冲宽度调制 (PWM) 
- 串行端口

### <a name="iotdevicebindings"></a>Iot.Device.Bindings

`Iot.Device.Bindings`包：

* 包含 [设备绑定](https://github.com/dotnet/iot/blob/master/src/devices/README.md) <span class="docon docon-navigate-external x-hidden-focus"></span> ，用于通过包装 system.web 来简化应用程序开发。
* 支持社区，并持续添加其他绑定。

常用的设备绑定包括：

- [CharacterLcd-LCD 字符显示](https://github.com/dotnet/iot/tree/master/src/devices/CharacterLcd)<span class="docon docon-navigate-external x-hidden-focus"></span>
- [SN74HC595-8 位移位寄存器](https://github.com/dotnet/iot/tree/master/src/devices/Sn74hc595)<span class="docon docon-navigate-external x-hidden-focus"></span>
- [BrickPi3](https://github.com/dotnet/iot/tree/master/src/devices/BrickPi3)<span class="docon docon-navigate-external x-hidden-focus"></span>
- [Max7219 导向 Matrix 驱动程序](https://github.com/dotnet/iot/tree/master/src/devices/Max7219)<span class="docon docon-navigate-external x-hidden-focus"></span>
- [RGBLedMatrix-RGB LED 矩阵](https://github.com/dotnet/iot/tree/master/src/devices/RGBLedMatrix)<span class="docon docon-navigate-external x-hidden-focus"></span>

## <a name="supported-operating-systems"></a>支持的操作系统

`System.Device.Gpio` 支持 ARM/ARM64 和 Windows 10 IoT Core 的大多数 Linux 版本都支持。

> [!TIP]
> 对于 Raspberry Pi，建议使用[Raspberry PI OS](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) <span class="docon docon-navigate-external x-hidden-focus"></span> (以前的 Raspbian) 。  

## <a name="supported-hardware-platforms"></a>支持的硬件平台

`System.Device.Gpio` 与大多数单板平台兼容。 推荐的平台是 Raspberry Pi (2 及更高版本) 和 Hummingboard。 已知兼容的其他平台是 BeagleBoard 和 ODROID。

通过使用 USB 到 SPI/I2C 桥，支持 PC 平台。

> [!IMPORTANT]
> 在 ARMv6 体系结构设备上不支持 .NET，其中包括 Raspberry Pi 2 之前的 Raspberry Pi 零和 Raspberry Pi 设备。

## <a name="resources"></a>资源

- [Github 上的 .Net IoT 库](https://github.com/dotnet/iot)<span class="docon docon-navigate-external x-hidden-focus"></span>
