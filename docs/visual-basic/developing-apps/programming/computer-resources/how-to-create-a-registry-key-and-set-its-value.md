---
title: 如何：创建注册表项并设置其值
ms.date: 07/20/2015
f1_keywords:
- RegistryKey.CreateSubKey
- RegistryKey.SetValue
helpviewer_keywords:
- registry keys [Visual Basic], creating
- registry [Visual Basic], adding values
- registry [Visual Basic], adding keys
- registry keys [Visual Basic], setting values
- examples [Visual Basic], registry
ms.assetid: d3e40f74-c283-480c-ab18-e5e9052cd814
ms.openlocfilehash: b51a14e5e9c69078330f5b2161f74cff8e4da332
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363443"
---
# <a name="how-to-create-a-registry-key-and-set-its-value-in-visual-basic"></a>如何：在 Visual Basic 中创建注册表项并设置其值

`My.Computer.Registry` 对象的 `CreateSubKey` 方法可用于创建注册表项。

## <a name="procedure"></a>过程

### <a name="to-create-a-registry-key"></a>创建注册表项

- 使用 `CreateSubKey` 方法，指定放置注册表项的配置单元以及注册表项的名称。 参数 `Subkey` 不区分大小写。 此示例在 HKEY_CURRENT_USER 下创建注册表项 `MyTestKey`。

    [!code-vb[VbResourceTasks#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#17)]

### <a name="to-create-a-registry-key-and-set-a-value-in-it"></a>创建注册表项并设置其值

1. 使用 `CreateSubkey` 方法，指定放置注册表项的配置单元以及注册表项的名称。 此示例在 HKEY_CURRENT_USER 下创建注册表项 `MyTestKey`。

    [!code-vb[VbResourceTasks#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#17)]

2. 使用 `SetValue` 方法设置值。 此示例设置字符串值。 “MyTestKeyValue”设置为“This is a test value”。

    [!code-vb[VbResourceTasks#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#14)]

## <a name="example"></a>示例

此示例在 HKEY_CURRENT_USER 下创建注册表项 `MyTestKey`，然后将字符串值 `MyTestKeyValue` 设置为 `This is a test value`。

[!code-vb[VbResourceTasks#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#15)]

## <a name="robust-programming"></a>可靠编程

检查注册表结构，查找适合项的位置。 例如，可能需要打开当前用户的 HKEY_CURRENT_USER\Software 项，并用公司的名称创建一项。 然后将注册表值添加到公司的项上。

从 Web 应用程序读取注册表时，当前用户依赖于在 Web 应用程序中实现的身份验证和模拟。

将数据写入用户文件夹 (<xref:Microsoft.Win32.Registry.CurrentUser>) 比写入本地计算机 (<xref:Microsoft.Win32.Registry.LocalMachine>) 更安全。

创建注册表值时，需要确定该值已存在时应执行的操作。 另一进程（可能是恶意进程）可能已创建了该值，并拥有对该值的访问权。 将数据放入注册表值后，其他进程即可使用这些数据。 若要防止出现这种情况，请使用 <xref:Microsoft.Win32.RegistryKey.GetValue%2A> 方法。 如果项不存在，则该方法返回 `Nothing`。

即使注册表项受 ACL（访问控制列表）保护，在注册表中以纯文本形式存储机密信息（例如密码）也不安全。

以下情况可能会导致异常：

- 密钥名称是 `Nothing` (<xref:System.ArgumentNullException>)。

- 用户没有创建注册表项的权限 (<xref:System.Security.SecurityException>)。

- 项名称超过 255 个字符的限制 (<xref:System.ArgumentException>)。

- 项已关闭 (<xref:System.IO.IOException>)。

- 注册表项为只读 (<xref:System.UnauthorizedAccessException>)。

## <a name="net-framework-security"></a>.NET Framework 安全性

若要运行此进程，程序集需要 <xref:System.Security.Permissions.RegistryPermission> 类授予的特权等级。 如果在部分信任上下文中运行，该进程可能会因特权不足而引发异常。 同样，用户必须具有用于创建或写入设置的正确 ACL。 例如，具有代码访问安全性权限的本地应用程序可能没有操作系统权限。 有关详细信息，请参阅[代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy.CurrentUser%2A>
- <xref:Microsoft.Win32.RegistryKey.CreateSubKey%2A>
- [读取和写入注册表](reading-from-and-writing-to-the-registry.md)
- [代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)
