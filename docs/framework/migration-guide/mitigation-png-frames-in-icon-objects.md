---
title: 缓解：图标对象中的 PNG 帧
description: 了解在 .NET Framework 4.6 及更高版本中包含新行为不可取时，要如何配置图标对象中 PNG 帧的行为。
ms.date: 03/30/2017
ms.assetid: ca87fefb-7144-4b4e-8832-5a939adbb4b2
ms.openlocfilehash: a8bb4fca19a09f16c89ce8c5da08ebcae9a037ec
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276576"
---
# <a name="mitigation-png-frames-in-icon-objects"></a>缓解：图标对象中的 PNG 帧

从 .NET Framework 4.6 开始， <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> 方法成功将带 PNG 帧的图标转换为 <xref:System.Drawing.Bitmap> 对象。  
  
 在面向 .NET Framework 4.5.2 和更早版本的应用中， <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> 方法在 <xref:System.ArgumentOutOfRangeException> 对象具有 PNG 帧时引发 <xref:System.Drawing.Icon> 异常。  
  
## <a name="impact"></a>影响  

 此更改会影响以下应用：重新编译为面向 .NET Framework 4.6 的应用，以及对在 <xref:System.ArgumentOutOfRangeException> 对象具有 PNG 帧时引发的 <xref:System.Drawing.Icon> 实施特殊处理的应用。 在.NET Framework 4.6 下运行时，转换成功，不再引发 <xref:System.ArgumentOutOfRangeException> ，因此不再调用异常处理程序。  
  
### <a name="mitigation"></a>缓解  

 如果此行为不可取，可以在 app.config 文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分中添加下面的元素来保留旧行为：  
  
```xml  
<AppContextSwitchOverrides
      value="Switch.System.Drawing.DontSupportPngFramesInIcons=true" />  
```  
  
 如果 app.config 文件中已包含 `AppContextSwitchOverrides` 元素，新值应与 `value` 特性合并，如下所示：  
  
```xml  
<AppContextSwitchOverrides
      value="Switch.System.Drawing.DontSupportPngFramesInIcons=true;previous key=previous-value" />
```
  
## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
