---
title: 如何：下载文件
ms.date: 07/20/2015
helpviewer_keywords:
- downloading Internet resources [Visual Basic], files
- downloading files [Visual Basic]
- remote computers [Visual Basic], downloading from
- files [Visual Basic], downloading
- files [Visual Basic], transferring
ms.assetid: ac479f81-c0e2-4b99-af73-217f446b73da
ms.openlocfilehash: a5b379da00656f65476e4d9504457bf8b464beac
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411650"
---
# <a name="how-to-download-a-file-in-visual-basic"></a>如何：在 Visual Basic 中下载文件

<xref:Microsoft.VisualBasic.Devices.Network.DownloadFile%2A> 方法可用来下载远程文件并将其存储到特定位置。 如果 `ShowUI` 参数设置为 `True`，则显示一个对话框，该对话框显示下载进度并允许用户取消该操作。 默认情况下，不会覆盖同名的现有文件；如果希望覆盖现有文件，则将 `overwrite` 参数设为 `True`。

以下情况可能会导致异常：

- 驱动器名称无效 (<xref:System.ArgumentException>)。

- 尚未提供必要的身份验证（<xref:System.UnauthorizedAccessException> 或 <xref:System.Security.SecurityException>）。

- 服务器未在指定的 `connectionTimeout` (<xref:System.TimeoutException>) 内响应。

- 请求被网站 (<xref:System.Net.WebException>) 拒绝。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

> [!IMPORTANT]
> 不要根据文件的名称来判断文件的内容。 例如，文件 Form1.vb 可能不是 Visual Basic 源文件。 在应用程序中使用输入的数据之前，需验证所有的输入内容。 文件的内容可能不是预期内容，并且用来读取该文件的方法可能失败。

### <a name="to-download-a-file"></a>下载文件

- 使用 `DownloadFile` 方法下载文件，同时将目标文件的位置指定为字符串或 URI 并指定要存储该文件的位置。 此示例从 `http://www.cohowinery.com/downloads` 下载 `WineList.txt` 文件，并将其保存到 `C:\Documents and Settings\All Users\Documents` 中：

  [!code-vb[VbResourceTasks#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#9)]

### <a name="to-download-a-file-specifying-a-time-out-interval"></a>下载文件，并指定超时间隔

- 使用 `DownloadFile` 方法下载文件，同时将目标文件的位置指定为字符串或 URI，指定要存储该文件的位置，并以毫秒为单位指定超时间隔（默认值为 1000 毫秒）。 此示例从 `http://www.cohowinery.com/downloads` 下载 `WineList.txt` 文件，然后将该文件保存到 `C:\Documents and Settings\All Users\Documents`，同时将超时间隔指定为 500 毫秒：

  [!code-vb[VbResourceTasks#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#10)]

### <a name="to-download-a-file-supplying-a-user-name-and-password"></a>提供用户名和密码下载文件

- 使用 `DownLoadFile` 方法下载文件，同时将目标文件的位置指定为字符串或 URI，并指定要存储该文件的位置、用户名和密码。 此示例使用用户名 `anonymous` 和空密码从 `http://www.cohowinery.com/downloads` 下载 `WineList.txt` 文件，然后将该文件保存到 `C:\Documents and Settings\All Users\Documents`。

  [!code-vb[VbResourceTasks#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#11)]

  > [!IMPORTANT]
  > `DownLoadFile` 方法使用的 FTP 协议以纯文本方式发送信息（包括密码），因此不应用于传送敏感信息。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Network>
- <xref:Microsoft.VisualBasic.Devices.Network.DownloadFile%2A>
- [如何：上载文件](how-to-upload-a-file.md)
- [如何：分析文件路径](../drives-directories-files/how-to-parse-file-paths.md)
