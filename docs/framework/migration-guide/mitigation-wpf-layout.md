---
title: 缓解：WPF 布局
description: 了解如何缓解由 WPF 控件布局的更改导致的问题（例如，将对象移动到一个像素的位置）。
ms.date: 03/30/2017
ms.assetid: 805ffd7f-8d1e-427e-a648-601ca8ec37a5
ms.openlocfilehash: caed758f6e86a1e2bee255c2f29ca3160b327e17
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244075"
---
# <a name="mitigation-wpf-layout"></a>缓解：WPF 布局

WPF 控件的布局可能稍有变化。  
  
## <a name="impact"></a>影响  

 此更改的结果是：  
  
- 元素的宽度或高度最多可以扩大或收缩一个像素。  
  
- 对象的位置最多可以移动一个像素。  
  
- 居中的元素最多可以垂直或水平地偏离中心一个像素。  
  
 默认情况下，仅对面向 .NET Framework 4.6 的应用启用此新布局。  
  
## <a name="mitigation"></a>缓解  

 由于这种修改往往会消除高 DPI 处 WPF 控件的右侧或底部剪辑，因此面向早期版本的 .NET framework 但在.NET Framework 4.6 上运行的应用可以通过将下面的行添加到 app.config 文件的 `<runtime>` 部分来选择加入此新行为：  
  
```xml  
<AppContextSwitchOverrides value="Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false" />  
```  
  
 面向 .NET Framework 4.6 但希望 WPF 控件使用之前的布局算法来呈现的应用可以通过将下面的行添加到 app.config 文件的 `<runtime>` 部分来执行此操作：  
  
```xml  
<AppContextSwitchOverrides value="Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=true" />  
```  
  
## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
