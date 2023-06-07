---
title: 播放声音
ms.date: 07/20/2015
helpviewer_keywords:
- system sounds, playing
- system sounds
- playing sounds, Visual Basic
- sound loops
- My.Computer.Audio object, tasks
- sounds, playing
- sounds, background
- playing sounds
ms.assetid: f0d9e4ab-57c7-47b6-86d3-99ff07078040
ms.openlocfilehash: 416fedd011ff35d2b32d1b64932e3908a73ed14e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345524"
---
# <a name="playing-sounds-visual-basic"></a>播放声音 (Visual Basic)

`My.Computer.Audio` 对象提供用于播放声音的方法。  
  
## <a name="playing-sounds"></a>播放声音  

 应用程序可通过背景播放在播放声音时执行其他代码。 `My.Computer.Audio.Play` 方法允许应用程序一次仅播放一种背景声音；应用程序播放新背景声音时，会停止播放上一种背景声音。 你也可以播放一种声音并等待其播放完毕。  
  
 下例中使用 `My.Computer.Audio.Play` 方法播放声音。 指定 `AudioPlayMode.WaitToComplete` 时，`My.Computer.Audio.Play` 将等待声音播放完毕，然后再调用代码继续。 使用此示例时，应确保文件名可指代计算机中的 .wav 声音文件  
  
 [!code-vb[VbVbalrMyComputer#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#15)]  
  
 下例中使用 `My.Computer.Audio.Play` 方法播放声音。 使用此示例时，应确保应用程序资源包含名为 Waterfall 的 .wav 声音文件。  
  
 [!code-vb[VbVbalrMyComputer#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#16)]  
  
## <a name="playing-looping-sounds"></a>播放循环声音  

 在下例中，指定 `PlayMode.BackgroundLoop` 时，`My.Computer.Audio.Play` 方法将播放指定的背景声音。 使用此示例时，应确保文件名可指代计算机中的 .wav 声音文件。  
  
 [!code-vb[VbVbalrMyComputer#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#11)]  
  
 在下例中，指定 `PlayMode.BackgroundLoop` 时，`My.Computer.Audio.Play` 方法将播放指定的背景声音。 使用此示例时，应确保应用程序资源包含名为 Waterfall 的 .wav 声音文件。  
  
 [!code-vb[VbVbalrMyComputer#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#12)]  
  
 前面的代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，它位于“Windows 窗体应用程序”>“声音”中。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
 一般情况下，应用程序播放循环声音时，声音最终将停止。  
  
## <a name="stopping-the-playing-of-sounds-in-the-background"></a>停止播放背景声音  

 使用 `My.Computer.Audio.Stop` 方法可停止应用程序当前播放的背景声音或循环声音。  
  
 一般情况下，应用程序播放循环声音时，声音将在某个时间点停止。  
  
 下面的示例将停止播放背景声音。  
  
 [!code-vb[VbVbalrMyComputer#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#18)]  
  
 前面的代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，它位于“Windows 窗体应用程序”>“声音”中。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="playing-system-sounds"></a>播放系统声音  

 使用 `My.Computer.Audio.PlaySystemSound` 方法可播放指定的系统声音。  
  
 `My.Computer.Audio.PlaySystemSound` 方法将作为 <xref:System.Media.SystemSound> 类中的共享成员之一的参数。 系统声音 <xref:System.Media.SystemSounds.Asterisk%2A> 通常表示错误。  
  
 下例使用 `My.Computer.Audio.PlaySystemSound` 方法播放系统声音。  
  
 [!code-vb[VbVbalrMyComputer#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Audio>
- <xref:Microsoft.VisualBasic.Devices.Audio.Play%2A>
- <xref:Microsoft.VisualBasic.Devices.Audio.PlaySystemSound%2A>
- <xref:Microsoft.VisualBasic.Devices.Audio.Stop%2A>
- <xref:Microsoft.VisualBasic.AudioPlayMode>
