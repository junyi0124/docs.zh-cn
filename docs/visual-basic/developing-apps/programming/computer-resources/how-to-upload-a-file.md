---
title: 如何：上载文件
ms.date: 07/20/2015
helpviewer_keywords:
- networks, uploading files
- files [Visual Basic], uploading
- uploading files [Visual Basic]
- UploadFile method [Visual Basic]
- My.Computer.Network.UploadFile method
ms.assetid: a8b37924-c523-4fd3-b5ca-cb0074df29cd
ms.openlocfilehash: cee6811d6b6d295c28eb683c5d2f7bcbb5fe94ab
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401805"
---
# <a name="how-to-upload-a-file-in-visual-basic"></a>如何：在 Visual Basic 中上传文件

可使用 <xref:Microsoft.VisualBasic.Devices.Network.UploadFile%2A> 方法上传文件并将文件存储到远程位置。 如果 `ShowUI` 参数设置为 `True`，则显示一个对话框，该对话框显示上传进度并允许用户取消该操作。  
  
### <a name="to-upload-a-file"></a>上传文件  
  
- 可以使用 `UploadFile` 方法上传文件，同时将源文件的位置和目标目录位置指定为字符串或 URI（统一资源标识符）。此示例将文件 `Order.txt` 上传到 `http://www.cohowinery.com/uploads.aspx`。  
  
     [!code-vb[VbResourceTasks#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#6)]  
  
### <a name="to-upload-a-file-and-show-the-progress-of-the-operation"></a>上传文件并显示操作进度  
  
- 可以使用 `UploadFile` 方法上传文件，同时将源文件的位置和目标目录位置指定为字符串或 URI。 此示例在不提供用户名或密码的情况下将文件 `Order.txt` 上传到 `http://www.cohowinery.com/uploads.aspx`显示了上传操作的进度，并将将超时间隔为 500 毫秒。  
  
     [!code-vb[VbResourceTasks#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#7)]  
  
### <a name="to-upload-a-file-supplying-a-user-name-and-password"></a>上传文件并提供用户名和密码  
  
- 可以使用 `UploadFile` 方法上传文件，同时将源文件的位置和目标目录位置指定为字符串或 URI，并指定用户名和密码。 此示例将文件 `Order.txt` 上传到 `http://www.cohowinery.com/uploads.aspx`，并提供了用户名 `anonymous` 和空白密码。  
  
     [!code-vb[VbResourceTasks#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#8)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会引发异常：  
  
- 本地文件路径无效 (<xref:System.ArgumentException>)。  
  
- 身份验证失败 (<xref:System.Security.SecurityException>)。  
  
- 连接超时 (<xref:System.TimeoutException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Network?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Devices.Network.UploadFile%2A>
- [如何：下载文件](how-to-download-a-file.md)
- [如何：分析文件路径](../drives-directories-files/how-to-parse-file-paths.md)
