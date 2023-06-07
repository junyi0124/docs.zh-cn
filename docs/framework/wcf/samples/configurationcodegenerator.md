---
title: ConfigurationCodeGenerator
ms.date: 03/30/2017
ms.assetid: 3913aae8-165f-4014-9262-7fe426f90cb2
ms.openlocfilehash: b8496992c7b0694a07ac047ba8537c67fc363c02
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264226"
---
# <a name="configurationcodegenerator"></a>ConfigurationCodeGenerator

ConfigurationCodeGenerator 是一个工具，使用该工具可以向配置系统公开您的自定义通道实现。 这使自定义通道的用户可以通过使用 .config 文件来配置您的通道，就像配置系统提供的绑定（如 `NetTcpBinding`）或使用 `TcpTransportBindingElement` 的自定义绑定一样。  
  
 当您编写自定义通道并使用新的 `BindingElement` 或 `Binding` 将其公开给编程模型时，必须创建一组类，以使 `BindingElement` 或 `Binding` 能够使用 .config 文件进行配置。 您可以使用 ConfigurationCodeGenerator 工具生成这些类，并改善您的客户体验。  
  
### <a name="to-build-the-tool"></a>生成工具  
  
1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
2. 生成解决方案将生成一个文件：ConfigurationCodeGenerator.exe。 文件 Samplerun.cmd 提供了一个示例命令行，说明如何使用此工具生成传输的类 [： UDP](transport-udp.md) 示例。  
  
### <a name="to-run-the-tool"></a>运行此工具  
  
1. 如果您同时具有自定义 `BindingElement` 类型和自定义 `Binding` 类型，请在命令提示符下键入以下内容：  
  
    ```console  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereTheseTypesAreDefined  
    ```  
  
     如果只有自定义 `BindingElement` 类型，请键入以下内容：  
  
    ```console  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /dll: TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     如果只有自定义 `Binding` 类型，请键入以下内容：  
  
    ```console  
    ConfigurationCodeGenerator.exe /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     该命令将为 `BindingElement` 生成三个 .cs 文件（如果您指定了 /be: 选项），为标准 `Binding` 生成五个 .cs 文件（如果您指定了 /sb: 选项）以及一个 .xml 文件。  
  
    1. 如果您使用了 /be 选项，其中一个 .cs 文件将为您的绑定元素实现 `BindingElementExtensionSection`。 此代码将您的 `BindingElement` 公开给配置系统，从而使其他自定义绑定可以使用您的绑定元素。 其他文件中包含代表默认值和常量的类。 这些文件中包含 `//TODO` 注释，用于提醒您更新默认值。  
  
    2. 如果你指定了 /sb 选项，两个 .cs 文件将分别实现 `StandardBindingElement` 和 `StandardBindingCollectionElement`，从而将你的标准绑定公开给配置系统。 其他文件中包含代表默认值和常量的类。 这些文件中包含 `//TODO` 注释，用于提醒您更新默认值。  
  
         如果指定了/sb：选项，则 Codetoaddto< \<*YourStdBinding*> 包含必须手动添加到实现标准绑定的类中的代码。  
  
     必须将 SampleConfig.xml 文件中包含的配置代码添加到注册前面步骤 1 或步骤 2 中定义的处理程序的配置文件中。  
