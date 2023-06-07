---
ms.openlocfilehash: 85f50b221e7ecb1ebd6fa539894ab7aabed8d362
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "67540080"
---
### <a name="install-the-global-tool"></a>安装全局工具

应使用 `--tool-path` 选项将包资产安装在受保护的位置。 这种分离可避免与提升的环境共享受限的用户环境。

```bash
sudo dotnet tool install PACKAGEID --tool-path /usr/local/share/dotnet-tools
```

将使用权限 `drwxr-xr-x` 创建 `/usr/local/share/dotnet-tools`。 如果该目录已存在，请使用 `ls -l` 命令验证受限的用户是否无权编辑该目录。 如果是，请使用 `sudo chmod o-w -R /usr/share/dotnet-tools` 命令删除访问权限。

### <a name="run-the-global-tool"></a>运行全局工具

**选项 1** 对 sudo 使用完整路径：

```bash
sudo /usr/local/share/dotnet-tools/TOOLCOMMAND
```

**选项 2** 为每个工具添加一次工具的符号链接：

```bash
sudo ln -s /usr/local/share/dotnet-tools/TOOLCOMMAND /usr/local/bin/TOOLCOMMAND
```

然后使用以下命令运行：

```bash
sudo TOOLCOMMAND
```

### <a name="uninstall-the-global-tool"></a>卸载全局工具

```bash
sudo dotnet tool uninstall PACKAGEID --tool-path /usr/local/share/dotnet-tools
```

如果创建了符号链接，还需将其删除：

```bash
sudo rm /usr/local/bin/TOOLCOMMAND
```
