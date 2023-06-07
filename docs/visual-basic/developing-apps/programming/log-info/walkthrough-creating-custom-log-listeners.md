---
title: 创建自定义日志侦听器
ms.date: 07/20/2015
helpviewer_keywords:
- custom log listeners
- My.Application.Log object, custom log listeners
ms.assetid: 0e019115-4b25-4820-afb1-af8c6e391698
ms.openlocfilehash: 5a140607a4fe7e1e13de54e8d56cab53e52aaa2a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398261"
---
# <a name="walkthrough-creating-custom-log-listeners-visual-basic"></a>演练：创建自定义日志侦听器 (Visual Basic)

本演练演示如何创建自定义日志侦听器，并将其配置为侦听 `My.Application.Log` 对象的输出。

## <a name="getting-started"></a>入门

日志侦听器必须继承自 <xref:System.Diagnostics.TraceListener> 类。

#### <a name="to-create-the-listener"></a>创建侦听器

- 在应用程序中，创建继承自 <xref:System.Diagnostics.TraceListener> 的类，其名称为 `SimpleListener`。

     [!code-vb[VbVbalrMyApplicationLog#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#16)]

     基类所需的 <xref:System.Diagnostics.TraceListener.Write%2A> 和 <xref:System.Diagnostics.TraceListener.WriteLine%2A> 方法调用 `MsgBox`，以显示其输入。

     <xref:System.Security.Permissions.HostProtectionAttribute> 特性应用于 <xref:System.Diagnostics.TraceListener.Write%2A> 和 <xref:System.Diagnostics.TraceListener.WriteLine%2A> 方法，使其特性与基类方法匹配。 <xref:System.Security.Permissions.HostProtectionAttribute> 特性使运行代码的主机能够确定代码会公开主机保护同步。

    > [!NOTE]
    > <xref:System.Security.Permissions.HostProtectionAttribute> 特性仅在可托管公共语言运行时且实现主机保护的非托管应用程序上有效，如 SQL Server。

若要确保 `My.Application.Log` 使用日志侦听器，应对包含日志侦听器的程序集执行强名称。

接下来的过程提供一些用于创建强名称日志侦听器程序集的简单步骤。 有关详细信息，请参阅[创建和使用具有强名称的程序集](../../../../standard/assembly/create-use-strong-named.md)。

#### <a name="to-strongly-name-the-log-listener-assembly"></a>对日志侦听器程序集执行强名称

1. 在 **“解决方案资源管理器”** 中选择一个项目。 在 **“项目”** 菜单上，选择 **“属性”** 。

2. 单击“签名”选项卡。

3. 选择“为程序集签名”框。

4. 在“选择强名称密钥文件”下拉列表中，选择“\<New>”。 

     将打开“创建强名称密钥”对话框。

5. 在“密钥文件名”框中提供密钥文件的名称。

6. 在“输入密码”和“确认密码”框中输入密码。 

7. 单击“确定”。

8. 重新生成应用程序。

## <a name="adding-the-listener"></a>添加侦听器

现在程序集已具有强名称，需要确定侦听器的强名称，以便 `My.Application.Log` 使用日志侦听器。

强名称类型的格式如下所示。

\<type name>, \<assembly name>, \<version number>, \<culture>, \<strong name>

#### <a name="to-determine-the-strong-name-of-the-listener"></a>确定侦听器的强名称

- 以下代码演示如何确定 `SimpleListener` 的强名称类型名称。

     [!code-vb[VbVbalrMyApplicationLog#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#17)]

     该类型的强名称取决于项目。

借助强名称，可以将侦听器添加到 `My.Application.Log` 日志侦听器集合中。

#### <a name="to-add-the-listener-to-myapplicationlog"></a>将侦听器添加到 My.Application.Log

1. 在“解决方案资源管理器”中右键单击 app.config，然后选择“打开”。 

     \- 或 -

     如果其中有 app.config 文件：

    1. 在 **“项目”** 菜单上选择 **“添加新项”** 。

    2. 在“添加新项”  对话框中，选择“应用程序配置文件” 。

    3. 单击 **添加**。

2. 找到 `<listeners>` 部分，该部分位于 `<source>` 属性为“DefaultSource”的 `name` 部分当中，后者又位于 `<sources>` 部分之下。 `<sources>` 部分位于 `<system.diagnostics>` 部分当中，后者又位于顶级 `<configuration>` 部分之下。

3. 将此元素添加到 `<listeners>` 部分：

    ```xml
    <add name="SimpleLog" />
    ```

4. 找到 `<sharedListeners>` 部分，该部分位于 `<system.diagnostics>` 部分当中，后者又位于顶级 `<configuration>` 部分之下。

5. 将此元素添加到该 `<sharedListeners>` 部分：

    ```xml
    <add name="SimpleLog" type="SimpleLogStrongName" />
    ```

     将 `SimpleLogStrongName` 的值更改为该侦听器的强名称。

## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- [使用应用程序日志](working-with-application-logs.md)
- [如何：日志异常](how-to-log-exceptions.md)
- [如何：写入日志消息](how-to-write-log-messages.md)
- [演练：更改 My.Application.Log 在哪里写入信息](walkthrough-changing-where-my-application-log-writes-information.md)
