---
title: 如何：创建用户设置的属性网格
ms.date: 07/20/2015
helpviewer_keywords:
- My.Settings object [Visual Basic], creating property grids for user settings
- user settings [Visual Basic], creating property grids
- property grids [Visual Basic], creating for user settings
- property grids
ms.assetid: b0bc737e-50d1-43d1-a6df-268db6e6f91c
ms.openlocfilehash: e93c62ad138be260422319e28a3ed85dd1871a1b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410161"
---
# <a name="how-to-create-property-grids-for-user-settings-in-visual-basic"></a>如何：在 Visual Basic 中为用户设置创建属性网格

可通过使用 `My.Settings` 对象的用户设置属性填充 <xref:System.Windows.Forms.PropertyGrid> 控件，创建用户设置的属性网格。  
  
> [!NOTE]
> 若要使此示例正确运行，应用程序必须配置用户设置。 有关详细信息，请参阅[管理应用程序设置 (.NET)](/visualstudio/ide/managing-application-settings-dotnet)。  
  
 `My.Settings` 对象将每个设置公开为一个属性。 属性名称就是设置的名称，属性类型就是设置类型。 设置的“范围”确定属性是否为只读；“应用程序”范围设置的属性为只读，而“用户”范围设置的属性为读写。    有关详细信息，请参阅 [My.Settings 对象](../../../language-reference/objects/my-settings-object.md)。  
  
> [!NOTE]
> 不能在运行时更改或保存应用程序范围设置的值。 只有在创建应用程序（通过“项目设计器”）或编辑应用程序的配置文件时才能更改应用程序范围设置。  有关详细信息，请参阅[管理应用程序设置 (.NET)](/visualstudio/ide/managing-application-settings-dotnet)。  
  
 此示例使用 <xref:System.Windows.Forms.PropertyGrid> 控件访问 `My.Settings` 对象的用户设置属性。 默认情况下，<xref:System.Windows.Forms.PropertyGrid> 显示 `My.Settings` 对象的所有属性。 但是，用户设置属性具有 <xref:System.Configuration.UserScopedSettingAttribute> 特性。 此示例将 <xref:System.Windows.Forms.PropertyGrid> 的 <xref:System.Windows.Forms.PropertyGrid.BrowsableAttributes%2A> 属性设置为 <xref:System.Configuration.UserScopedSettingAttribute>，以仅显示用户设置属性。  
  
### <a name="to-add-a-user-setting-property-grid"></a>添加用户设置属性网格  
  
1. 将“PropertyGrid”控件从“工具箱”添加到应用程序的设计图面上（假定此处为 `Form1`）。    
  
     属性网格控件的默认名称为 `PropertyGrid1`。  
  
2. 双击 `Form1` 的设计图面打开窗体加载事件处理程序的代码。  
  
3. 将 `My.Settings` 对象设置为属性网格的选定对象。  
  
     [!code-vb[VbVbalrMyResources#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#11)]  
  
4. 将属性网格配置为只显示用户设置。  
  
     [!code-vb[VbVbalrMyResources#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#12)]  
  
    > [!NOTE]
    > 若要只显示应用程序范围设置，请使用 <xref:System.Configuration.ApplicationScopedSettingAttribute> 特性而不是 <xref:System.Configuration.UserScopedSettingAttribute>。  
  
## <a name="robust-programming"></a>可靠编程  

 应用程序在关闭时会保存用户设置。 若要立即保存设置，请调用 `My.Settings.Save` 方法。 有关详细信息，请参阅[如何：在 Visual Basic 中暂留用户设置](how-to-persist-user-settings.md)。  
  
## <a name="see-also"></a>请参阅

- [My.Settings 对象](../../../language-reference/objects/my-settings-object.md)
- [如何：在 Visual Basic 中读取应用程序设置](how-to-read-application-settings.md)
- [如何：在 Visual Basic 中更改用户设置](how-to-change-user-settings.md)
- [如何：在 Visual Basic 中暂留用户设置](how-to-persist-user-settings.md)
- [管理应用程序设置 (.NET)](/visualstudio/ide/managing-application-settings-dotnet)
