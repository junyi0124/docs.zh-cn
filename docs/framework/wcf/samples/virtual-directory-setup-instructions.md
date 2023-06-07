---
title: 虚拟目录设置说明
ms.date: 03/30/2017
ms.assetid: 3c62cab5-81a4-48b6-ac8c-9ce33a85a157
ms.openlocfilehash: dba6547888935ccf36ec0924fd3c95e8fbda5688
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243646"
---
# <a name="virtual-directory-setup-instructions"></a>虚拟目录设置说明

Windows Communication Foundation (WCF) 示例旨在共享一个名为 servicemodelsamples 的公共虚拟目录，该目录映射到%SystemDrive%\inetpub\wwwroot\servicemodelsamples 文件夹。  
  
> [!NOTE]
> %SystemDrive% 通常为 C: 或 D:，具体取决于安装 Internet 信息服务 (IIS) 的驱动器位置。  
  
 你可以从 [Windows Communication Foundation "一次性" 设置过程](one-time-setup-procedure-for-the-wcf-samples.md) 运行 Setupvroot.bat 和 Cleanupvroot.bat 文件，以便创建虚拟目录。 如果宁愿手动创建虚拟目录，请使用下面的过程。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-create-a-virtual-directory-in-iis-70-or-75"></a>在 IIS 7.0 或7.5 中创建虚拟目录  
  
1. 从 " **开始** " 菜单中，单击 " **运行**"，然后键入 **INETMGR** 以打开 IIS) MMC 管理单元 (的 Internet Information Services。  
  
2. 在左侧窗格中，展开包含计算机名称的节点，然后展开 " **站点** " 节点。  
  
3. 右键单击 " **默认** 网站"，然后选择 " **添加应用程序** " 以打开 " **添加应用程序" 窗口**。  
  
4. 在窗口中，键入 `servicemodelsamples` 作为要创建的虚拟目录的别名。  
  
5. 创建以下目录：%SystemDrive%\inetpub\wwwroot\servicemodelsamples  
  
6. 将物理路径设置为 %SystemDrive%\inetpub\wwwroot\servicemodelsamples。  大多数 WCF 示例在生成后都将服务可执行文件复制到此位置。  
  
7. 单击" **确定**"。 现在已为 WCF 示例创建 Web 应用程序。  
  
    > [!NOTE]
    > 此任务仅需执行一次，因为所有 WCF 示例都使用相同的 servicemodelsamples Web 应用程序。  
  
    > [!NOTE]
    > 在本文档中，术语`virtual directory`和 `Web application`是同义词。  
  
     除了创建虚拟目录以外，还必须设置其属性以使 WCF 服务能够运行。 有关详细信息，请参阅下文。  
  
#### <a name="to-create-a-virtual-directory-in-iis-51-or-60"></a>在 IIS 5.1 或 6.0 中创建虚拟目录  
  
1. 打开 "命令提示符" 窗口并键入 `start inetmgr` 以打开 IIS) MMC 管理单元 (的 Internet Information Services。  
  
2. 在左侧窗格中，展开包含计算机名称的节点，然后展开 **"网站" 节点。**  
  
3. 右键单击 " **默认** 网站"，然后选择 " **新建虚拟目录"，** 打开 "虚拟目录创建向导"。  
  
4. 在向导中，键入 `servicemodelsamples` 作为要创建的虚拟目录的别名。  
  
5. 将路径设置为 %SystemDrive%\inetpub\wwwroot\servicemodelsamples。 大多数 WCF 示例在生成后都将服务可执行文件复制到此位置。  
  
6. 单击“下一步”。  
  
7. 默认情况下，已选中以下复选框：  
  
    - **读取**  
  
    - **运行脚本(如 ASP)**  
  
8. 单击 " **下一步**"，然后单击 " **完成** " 以完成向导。  
  
    > [!NOTE]
    > 此任务仅需执行一次，因为所有 WCF 示例都使用相同的 servicemodelsamples 虚拟目录。  
  
#### <a name="to-set-additional-virtual-directory-properties-in-iis-70-or-75"></a>在 IIS 7.0 或7.5 中设置附加虚拟目录属性  
  
1. 单击 servicemodelsamples 节点。 窗口的底部列有两个视图。 如果尚未选择 " **功能视图** "，请选择它。  
  
2. 双击 **目录浏览** 条目。  
  
3. 在 "操作" 窗格中，选择 " **启用** " 选项。 这样，您将能够使用 Internet Explorer 访问目录的目录，这在调试服务时将很有帮助。  
  
 最后，您必须设置 servicemodelsamples 文件夹的安全属性，以允许其他人访问该文件夹。 有关详细信息，请参阅下文。  
  
#### <a name="to-set-additional-virtual-directory-properties-in-iis-51-or-60"></a>在 IIS 5.1 或 6.0 中设置附加虚拟目录属性  
  
1. 右键单击 "servicemodelsamples" 节点，然后单击 " **属性**"。  
  
2. 默认情况下，已选中以下复选框：  
  
    - **读取**  
  
    - **日志访问**  
  
    - **为此资源编制索引**  
  
3. 选中 " **目录浏览** " 复选框。 这样，您将能够使用 Internet Explorer 访问目录的目录，这在调试服务时将很有帮助。  
  
#### <a name="to-set-security-properties-of-the-folder-in-iis-70-or-75"></a>在 IIS 7.0 或7.5 中设置文件夹的安全属性  
  
1. 定位到 %SystemDrive%\inetpub\wwwroot\servicemodelsamples。  
  
2. 右键单击 servicemodelsamples 文件夹，然后单击 " **共享** " 或 " **共享**"。  
  
3. 单击 " **添加** " 按钮左侧的向下箭头。  
  
4. 选择 " **查找** " 条目。 此时将打开 " **选择用户或组** " 窗口。  
  
5. 单击“高级”。  
  
6. 单击“位置”。 此时会打开 " **位置** " 窗口。  
  
7. 选择对应于所使用计算机的项。 请务必选择本地计算机，而不是对应于所列出的任何域或网络的项。 选择计算机后，单击 **"确定"**。  
  
8. 单击“立即查找”。  此操作将用与本地计算机关联的对象填充搜索结果。  
  
9. 在 **名称 (相对可分辨名称)** 列中查找 **IIS_IUSRS** 条目。 选择该条目，然后单击 **"确定"** 以关闭 "搜索结果" 窗口。  
  
10. 单击 **"确定"** 以关闭 " **选择用户或组** " 窗口。  
  
11. 单击 " **共享** " 以保存更改。  
  
12. 完成启用共享的更改后，单击 " **完成** " 关闭 " **文件共享** " 窗口。  
  
#### <a name="to-set-security-properties-of-the-folder-in-iis-51-or-60"></a>在 IIS 5.1 或 6.0 中设置文件夹的安全属性  
  
1. 定位到 %SystemDrive%\inetpub\wwwroot\servicemodelsamples。  
  
2. 右键单击 **servicemodelsamples** 文件夹，然后单击 " **共享和安全"。**  
  
3. 单击“安全”选项卡。   
  
4. 如果使用的是 IIS 6.0，请在 " **组或用户名** " 框中检查是否列出了 " **Internet 来宾帐户** "。  
  
     如果未列出：  
  
    1. 单击“开始”，然后单击“控制面板” 。  
  
    2. 如果看不到 " **用户帐户** " 图标，请单击 " **切换到分类视图**"。  
  
    3. 单击 " **用户帐户** " 图标。  
  
    4. 在 "或选择一个控制面板图标下，单击" **用户帐户**"。  
  
    5. 在 " **用户帐户** " 对话框中，单击 " **高级** " 选项卡。  
  
    6. 单击“高级”。  
  
    7. 在 " **本地用户和组** " 对话框中，单击以展开 " **用户** " 文件夹。  
  
    8. 在右侧窗格中，双击 " **Internet 来宾帐户**"。  
  
    9. 在 " **属性** " 对话框中，复制用作 Internet 来宾帐户的名称。 默认情况下，该名称以“USR_”开头，后跟计算机的名称。  
  
    10. 关闭“属性”  对话框。  
  
    11. 关闭 " **本地用户和组** " 对话框。  
  
    12. 关闭 " **用户帐户** " 对话框。  
  
    13. 关闭 "其他 **用户帐户** " 对话框。  
  
    14. 在 " **Servicemodelsamples 属性** " 对话框中的 " **安全** " 选项卡上，单击 " **添加**"。  
  
    15. 键入后跟反斜杠的计算机名称，然后粘贴 Internet 用户帐户的名称，例如，myMachineName \\ % InternetGuestAccountName%  
  
    16. 单击 " **检查名称** " 以验证添加。 如果名称有效，名称将变为全大写并带下划线的形式。  
  
5. 对于 IIS 6.0，还要检查 " **组或用户名** " 框中是否列出了 "网络服务"。  
  
     如果“NETWORK SERVICE”未列出：  
  
    1. 单击“添加”。  
  
    2. 在 " **选择用户或组** " 对话框中，键入计算机名称，后跟反斜杠。  
  
    3. 在反斜杠后键入 **服务** (不) 空间。  
  
    4. 单击 " **检查名称**"。  
  
    5. 如果找到多个名称，请选择 " **网络服务** "，并单击 **"确定"**。  
  
    6. 单击 **"确定"** 以关闭 " **选择用户或组** " 对话框。  
  
6. 如果使用的是带有 IIS 5.1 的 Windows XP SP2，请检查 " **组名或用户名** " 框中是否列出了 "Internet 来宾帐户" 和 "ASPNET"。  
  
     请注意，ASPNET 用户可能是内置 **用户** 安全组的成员。 如果是这样，则在对话框中列出 " **用户** " 组时，无需将其作为单独的项添加到允许用户列表中。  
  
     若要检查是否是 **用户** 安全组的成员，请执行以下操作：  
  
    1. 在 **“开始”** 菜单上，单击 **“控制面板”** 。  
  
    2. 单击 " **用户帐户** " 图标。  
  
    3. 在 " **组** " 列中，检查 **ASPNET** 的值是否为 "用户"。  
  
## <a name="see-also"></a>另请参阅

- [Internet 信息服务承载说明](internet-information-service-hosting-instructions.md)
