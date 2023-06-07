---
title: 应用程序设置架构
ms.date: 03/30/2017
helpviewer_keywords:
- schema application settings
- application settings, schema [Windows Forms]
- Windows Forms, application settings schema
- configuration schema [.NET Framework], application settings
ms.assetid: 5797fcff-6081-4e8c-bebf-63d9c70cf14b
ms.openlocfilehash: fc9cd8ac3819c6a02019c871e7bd45ceb4c2cef7
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90552305"
---
# <a name="application-settings-schema"></a>应用程序设置架构

应用程序设置允许 Windows 窗体或 ASP.NET 应用程序存储和检索应用程序范围的设置和用户范围的设置。 在此上下文中， *设置* 是指特定于应用程序或特定于当前用户的任何信息，包括从数据库连接字符串到用户首选默认窗口大小的任何信息。

默认情况下，Windows 窗体应用程序中的应用程序设置使用 <xref:System.Configuration.LocalFileSettingsProvider> 类，该类使用 .net 配置系统将设置存储在 XML 配置文件中。 有关应用程序设置使用的文件的详细信息，请参阅 [应用程序设置体系结构](/dotnet/desktop/winforms/advanced/application-settings-architecture)。

应用程序设置将以下元素定义为它所使用的配置文件的一部分。

| 元素                    | 说明                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------- |
| **\<applicationSettings>** | 包含 **\<setting>** 应用程序特定的所有标记。                         |
| **\<userSettings>**        | 包含 **\<setting>** 特定于当前用户的所有标记。                        |
| **\<setting>**             | 定义设置。 或的子级 **\<applicationSettings>** **\<userSettings>** 。 |
| **\<value>**               | 定义设置的值。 的子项 **\<setting>** 。                                   |

## <a name="applicationsettings-element"></a>\<applicationSettings> 元素

此元素包含 **\<setting>** 特定于客户端计算机上的应用程序实例的所有标记。 未定义任何属性。

## <a name="usersettings-element"></a>\<userSettings> 元素

此元素包含 **\<setting>** 特定于当前正在使用应用程序的用户的所有标记。 未定义任何属性。

## <a name="setting-element"></a>\<setting> 元素

此元素定义一个设置。 它具有以下属性。

| Attribute        | 说明 |
| ---------------- | ----------- |
| name         | 必需。 设置的唯一 ID。 使用名称保存通过 Visual Studio 创建的设置 `ProjectName.Properties.Settings` 。 |
| **serializeAs** | 必需。 将值序列化为文本时要使用的格式。 有效值是：<br><br>- `string`. 该值使用来序列化为字符串 <xref:System.ComponentModel.TypeConverter> 。<br>- `xml`. 使用 XML 序列化对值进行序列化。<br>- `binary`. 使用二进制序列化将值作为文本编码的二进制序列化。<br />- `custom`. 设置提供程序具有此设置的固有知识，并对其进行序列化和反序列化。 |

## <a name="value-element"></a>\<value> 元素

此元素包含设置的值。

## <a name="example"></a>示例

以下示例显示了一个应用程序设置文件，该文件定义了两个应用程序范围的设置和两个用户范围的设置：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <sectionGroup name="applicationSettings" type="System.Configuration.ApplicationSettingsGroup, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
      <section name="WindowsApplication1.Properties.Settings" type="System.Configuration.ClientSettingsSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
    </sectionGroup>
    <sectionGroup name="userSettings" type="System.Configuration.UserSettingsGroup, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
      <section name="WindowsApplication1.Properties.Settings" type="System.Configuration.ClientSettingsSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" allowExeDefinition="MachineToLocalUser" />
    </sectionGroup>
  </configSections>
  <applicationSettings>
    <WindowsApplication1.Properties.Settings>
      <setting name="Cursor" serializeAs="String">
        <value>Default</value>
      </setting>
      <setting name="DoubleBuffering" serializeAs="String">
        <value>False</value>
      </setting>
    </WindowsApplication1.Properties.Settings>
  </applicationSettings>
  <userSettings>
    <WindowsApplication1.Properties.Settings>
      <setting name="FormTitle" serializeAs="String">
        <value>Form1</value>
      </setting>
      <setting name="FormSize" serializeAs="String">
        <value>595, 536</value>
      </setting>
    </WindowsApplication1.Properties.Settings>
  </userSettings>
</configuration>
```

## <a name="see-also"></a>请参阅

- [应用程序设置概述](/dotnet/desktop/winforms/advanced/application-settings-overview)
- [应用程序设置体系结构](/dotnet/desktop/winforms/advanced/application-settings-architecture)
