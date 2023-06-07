---
title: 如何：调用 Windows API
ms.date: 07/20/2015
helpviewer_keywords:
- API calls [Visual Basic]
- Windows API, calling
- API calls [Visual Basic], platform invoke
- calls [Visual Basic], stored procedures
ms.assetid: 27d75f0a-54ab-4ee1-b91d-43513a19b12d
ms.openlocfilehash: 40b40c1a489d514c82cbccdeacda27900d9ec87d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91083353"
---
# <a name="how-to-call-windows-apis-visual-basic"></a>如何：调用 Windows API (Visual Basic)

此示例 `MessageBox` 在 user32.dll 中定义并调用函数，然后将字符串传递到该函数。  
  
## <a name="example"></a>示例  

 [!code-vb[VbVbalrInterop#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#1)]  
  
## <a name="compile-the-code"></a>编译代码  

 此示例需要：  
  
- 对 <xref:System> 命名空间的引用。  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 方法不是静态的，也不是抽象的，或者以前定义过。 父类型为接口，或 *name* 或 *dllName* 的长度为零。 (<xref:System.ArgumentException>)  
  
- *名称*或*dllName*为 `Nothing` 。 (<xref:System.ArgumentNullException>)  
  
- 之前已使用 `CreateType` 创建包含类型。 (<xref:System.InvalidOperationException>)  
  
## <a name="see-also"></a>请参阅

- [平台调用详解](../../../framework/interop/consuming-unmanaged-dll-functions.md#a-closer-look-at-platform-invoke)
- [平台调用示例](../../../framework/interop/platform-invoke-examples.md)
- [使用非托管 DLL 函数](../../../framework/interop/consuming-unmanaged-dll-functions.md)
- [用反射发出定义方法](/previous-versions/dotnet/netframework-4.0/w63y4d4f(v=vs.100))
- [演练：调用 Windows API](walkthrough-calling-windows-apis.md)
- [COM 互操作](index.md)
