---
title: MsgBox 示例
description: 请参阅使用 MsgBox 通过值将字符串类型作为 In 参数传递的示例。 它演示了何时在 .NET 中使用 EntryPoint、CharSet 和 ExactSpelling 字段。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- marshaling, MsgBox sample
- data marshaling, MsgBox sample
ms.assetid: 9e0edff6-cc0d-4d5c-a445-aecf283d9c3a
ms.openlocfilehash: 024ed612115ce73646596651fe454238418446db
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242014"
---
# <a name="msgbox-sample"></a>MsgBox 示例

此示例演示如何通过值将字符串类型作为 In 参数传递，以及何时使用 <xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint>、<xref:System.Runtime.InteropServices.DllImportAttribute.CharSet> 和 <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling> 字段。  
  
 MsgBox 示例使用以下未托管的函数（与其原始函数声明一同显示）：  
  
- 从 User32.dll 导出的 MessageBox。  
  
    ```cpp
    int MessageBox(HWND hWnd, LPCTSTR lpText, LPCTSTR lpCaption,
       UINT uType);  
    ```  
  
 在此示例中，`NativeMethods` 类包含 `MsgBoxSample` 类调用的每一个未托管的函数的托管原型。 托管原型方法 `MsgBox``MsgBox2` 和 `MsgBox3` 对于相同的未托管的函数有不同的声明。  
  
 由于指定为 ANSI 的字符类型与 Unicode 函数名称的入口点 `MessageBoxW` 不匹配，`MsgBox2` 的声明会在消息框中生成不正确的输出。 `MsgBox3` 的声明会在 EntryPoint、CharSet 和 ExactSpelling 字段之间创建不匹配。 调用时，`MsgBox3` 会引发异常。 有关字符串命名和名称封送处理的详细信息，请参阅[指定字符集](specifying-a-character-set.md)。  
  
## <a name="declaring-prototypes"></a>声明原型  

 [!code-cpp[Conceptual.Interop.Marshaling#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/msgbox.cpp#5)]
 [!code-csharp[Conceptual.Interop.Marshaling#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/msgbox.cs#5)]
 [!code-vb[Conceptual.Interop.Marshaling#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/msgbox.vb#5)]  
  
## <a name="calling-functions"></a>调用函数  

 [!code-cpp[Conceptual.Interop.Marshaling#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/msgbox.cpp#6)]
 [!code-csharp[Conceptual.Interop.Marshaling#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/msgbox.cs#6)]
 [!code-vb[Conceptual.Interop.Marshaling#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/msgbox.vb#6)]  
  
## <a name="see-also"></a>请参阅

- [封送处理字符串](marshaling-strings.md)
- [字符串的默认封送处理](default-marshaling-for-strings.md)
- [在托管代码中创建原型](creating-prototypes-in-managed-code.md)
- [指定字符集](specifying-a-character-set.md)
