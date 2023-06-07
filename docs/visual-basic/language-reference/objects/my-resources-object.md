---
title: My.Resources 对象
ms.date: 07/20/2015
f1_keywords:
- My.Resources
- My.Resources.MyResources.ResourceManager
- My.Resources.MyResources.Culture
helpviewer_keywords:
- My.Resources object
ms.assetid: 34c3f2dc-7b87-432c-9d5f-17ea666bb266
ms.openlocfilehash: 3d12524706f680434d5b6d8da39c89042bea3281
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867337"
---
# <a name="myresources-object"></a>My.Resources 对象

提供用于访问应用程序资源的属性和类。  
  
## <a name="remarks"></a>备注  

 `My.Resources`对象提供对应用程序资源的访问权限，并允许您为应用程序动态检索资源。 有关详细信息，请参阅 [ ( .net) 管理应用程序资源 ](/visualstudio/ide/managing-application-resources-dotnet)。  
  
 `My.Resources` 对象只公开全局资源。 它不提供对与窗体关联的资源文件的访问权限。 必须从窗体访问窗体资源。  
  
 可以从对象访问应用程序的区域性特定资源文件 `My.Resources` 。 默认情况下， `My.Resources` 对象从与属性中的区域性匹配的资源文件中查找资源 <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.UICulture%2A> 。 不过，您可以重写此行为，并指定要用于资源的特定区域性。 有关详细信息，请参阅[桌面应用中的资源](../../../framework/resources/index.md)。  
  
## <a name="properties"></a>属性  

 对象的属性 `My.Resources` 提供对应用程序资源的只读访问。 若要添加或删除资源，请使用 " **项目设计器**"。 您可以使用资源中心访问通过**项目设计器**添加 `My.Resources.` *的资源*。  
  
 还可以通过在**解决方案资源管理器**中选择项目，然后在 "**项目**" 菜单中单击 "**添加新项**" 或 "**添加现有项**"，来添加或删除资源文件。 你可以使用 resourceFileName 资源中心访问以这种方式添加的资源 `My.Resources.` *resourceFileName* `.` *resourceName*。  
  
 每个资源都有一个名称、类别和值，这些资源设置决定了访问资源的属性在对象中的显示方式 `My.Resources` 。 对于在 **项目设计器**中添加的资源：  
  
- 名称确定属性的名称。  
  
- 资源数据是属性的值。  
  
- 类别确定属性的类型：  
  
|类别|属性数据类型|  
|---|---|  
|**字符串**|[字符串](../data-types/string-data-type.md)|  
|**映像**|<xref:System.Drawing.Bitmap>|  
|**图标**|<xref:System.Drawing.Icon>|  
|**音频**|<xref:System.IO.UnmanagedMemoryStream><br /><br /> <xref:System.IO.UnmanagedMemoryStream>类派生自 <xref:System.IO.Stream> 类，因此它可用于采用流的方法，例如 <xref:Microsoft.VisualBasic.Devices.Audio.Play%2A> 方法。|  
|**文件**|-   文本文件的[字符串](../data-types/string-data-type.md)。<br />-   <xref:System.Drawing.Bitmap> 用于图像文件。<br />-   <xref:System.Drawing.Icon> 图标文件。<br />-   <xref:System.IO.UnmanagedMemoryStream> 用于声音文件。|  
|**其他**|由设计器的 **类型** 列中的信息确定。|  
  
## <a name="classes"></a>类  

 `My.Resources`对象将每个资源文件作为具有共享属性的类公开。 类名称与资源文件的名称相同。 如上一节所述，资源文件中的资源作为类中的属性公开。  
  
## <a name="example"></a>示例  

 此示例将窗体的标题设置为 `Form1Title` 应用程序资源文件中名为的字符串资源。 要使此示例正常运行，应用程序的资源文件中必须具有一个名为的字符串 `Form1Title` 。  
  
 [!code-vb[VbVbalrMyResources#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#1)]  
  
## <a name="example"></a>示例  

 此示例将窗体的图标设置为 `Form1Icon` 存储在应用程序的资源文件中的名为的图标。 要使此示例正常运行，应用程序的资源文件中必须具有一个名为的图标 `Form1Icon` 。  
  
 [!code-vb[VbVbalrMyResources#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#2)]  
  
## <a name="example"></a>示例  

 此示例将窗体的背景图像设置为 `Form1Background` 应用程序资源文件中名为的图像资源。 要使此示例正常运行，应用程序的资源文件中必须具有一个名为的映像资源 `Form1Background` 。  
  
 [!code-vb[VbVbalrMyResources#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#3)]  
  
## <a name="example"></a>示例  

 此示例播放存储为 `Form1Greeting` 在应用程序的资源文件中名为的音频资源的声音。 要使此示例正常运行，应用程序的资源文件中必须具有一个名为的音频资源 `Form1Greeting` 。 `My.Computer.Audio.Play`方法仅可用于 Windows 窗体应用程序。  
  
 [!code-vb[VbVbalrMyResources#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#4)]  
  
## <a name="example"></a>示例  

 此示例检索应用程序的字符串资源的法语区域性版本。 资源命名为 `Message` 。 若要更改 `My.Resources` 对象使用的区域性，示例使用 <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.ChangeUICulture%2A> 。  
  
 要使此示例正常运行，应用程序的资源文件中必须具有一个名为的字符串 `Message` ，应用程序应具有该资源文件的法语区域性版本 Resources.fr。 如果应用程序没有资源文件的法语区域性版本，则 `My.Resource` 对象将从默认区域性资源文件中检索资源。  
  
 [!code-vb[VbVbalrMyResources#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#10)]  
  
## <a name="see-also"></a>另请参阅

- [管理应用程序资源 (.NET)](/visualstudio/ide/managing-application-resources-dotnet)
- [桌面应用中的资源](../../../framework/resources/index.md)
