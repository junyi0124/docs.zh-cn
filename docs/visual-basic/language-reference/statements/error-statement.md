---
title: Error 语句
ms.date: 07/20/2015
f1_keywords:
- vb.error
helpviewer_keywords:
- Error statement [Visual Basic], syntax
- Error statement [Visual Basic]
- Error keyword [Visual Basic]
- run-time errors [Visual Basic], codes
- errors [Visual Basic], simulating
ms.assetid: 85cd5c59-5224-4f02-aaf5-fcfefab17a29
ms.openlocfilehash: f3f9f5ecb96686fe525e98cf64672d81a3145796
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873272"
---
# <a name="error-statement"></a>Error 语句

模拟出现错误的情况。  
  
## <a name="syntax"></a>语法  
  
```vb  
Error errornumber  
```  
  
## <a name="parts"></a>组成部分  

 `errornumber`  
 必需。 可以是任何有效的错误号。  
  
## <a name="remarks"></a>备注  

 `Error`支持语句，以便向后兼容。 在新代码中，特别是在创建对象时，使用 `Err` 对象的 `Raise` 方法来生成运行时错误。  
  
 如果 `errornumber` 定义了，则在为 `Error` 对象的属性 `Err` 分配以下默认值之后，语句将调用错误处理程序：  
  
|属性|值|  
|--------------|-----------|  
|`Number`|指定为语句的参数的值 `Error` 。 可以是任何有效的错误号。|  
|`Source`|当前 Visual Basic 项目的名称。|  
|`Description`|对应于指定的函数的返回值的字符串表达式 `Error` `Number` （如果此字符串存在）。 如果字符串不存在，则 `Description` 包含零长度字符串 ( "" ) 。|  
|`HelpFile`|适当的 Visual Basic 帮助文件的完全限定驱动器、路径和文件名。|  
|`HelpContext`|对应于属性的错误的相应 Visual Basic 帮助文件上下文 ID `Number` 。|  
|`LastDLLError`|Zero。|  
  
 如果没有错误处理程序，或者未启用任何错误处理程序，则将创建错误消息并将其显示在 `Err` 对象属性中。  
  
> [!NOTE]
> 某些 Visual Basic 主机应用程序无法创建对象。 请参阅宿主应用程序的文档，以确定它是否可以创建类和对象。  
  
## <a name="example"></a>示例  

 此示例使用 `Error` 语句生成错误号11。  
  
```vb  
On Error Resume Next   ' Defer error handling.  
Error 11   ' Simulate the "Division by zero" error.  
```  
  
## <a name="requirements"></a>要求  

 **命名空间：** [Microsoft](../runtime-library-members.md)  
  
 **程序集：** Microsoft.VisualBasic.dll) 中的 Visual Basic 运行时库 (  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.ErrObject.Clear%2A>
- <xref:Microsoft.VisualBasic.Information.Err%2A>
- <xref:Microsoft.VisualBasic.ErrObject.Raise%2A>
- [On Error 语句](on-error-statement.md)
- [Resume 语句](resume-statement.md)
- [错误消息](../error-messages/index.md)
