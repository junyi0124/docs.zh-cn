---
title: 没有鼠标滚轮
ms.date: 07/20/2015
f1_keywords:
- vbrMouse_NoWheelIsPresent
ms.assetid: e924ffba-4af1-4247-9a6f-d19a03738f62
ms.openlocfilehash: a9b468d876945a177f3e326a7dc37e6c8a80285d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91078835"
---
# <a name="no-mouse-wheel-is-present"></a>没有鼠标滚轮

调用了 `My.Computer.Mouse.WheelScrollLines` 属性，但鼠标没有滚轮。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 检查 `My.Computer.Mouse.WheelExists` 属性，以查看鼠标是否有滚轮，然后再调用 `My.Computer.Mouse.WheelScrollLines` 属性。  
  
     \- 或 -  
  
- 在计算机上安装带滚轮的鼠标。  
  
## <a name="see-also"></a>请参阅

- [我的 My.computer.mouse.wheelscrolllines](xref:Microsoft.VisualBasic.Devices.Mouse.WheelScrollLines)
- [我的 My.computer.mouse.wheelexists](xref:Microsoft.VisualBasic.Devices.Mouse.WheelExists)
- [在 .NET 中处理和引发异常](../../standard/exceptions/index.md)
