---
title: 如何：获取 .NET Framework 4.5 安装程序的进度
description: 了解如何获取 .NET Framework 4.5 安装程序的进度。 如果要针对此 .NET 版本开发应用，可以在应用的安装程序中包含（链接）.NET Framework 4.5 安装程序。
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- progress information, .NET Framework installer
- .NET Framework, installing
ms.assetid: 0a1a3ba3-7e46-4df2-afd3-f3a8237e1c4f
ms.openlocfilehash: 7e21a376c5a7551ecadeaa70c0a70968dc5752fd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729117"
---
# <a name="how-to-get-progress-from-the-net-framework-45-installer"></a>如何：获取 .NET Framework 4.5 安装程序的进度

.NET Framework 4.5 是可再发行运行时。 如果开发基于此 .NET framework 版本的应用，则可以将 .NET Framework 4.5 安装程序作为必备组件包括（链接）在应用的安装程序中。 若要提供自定义或统一的安装体验，可能需要以无提示方式启动 .NET Framework 4.5 安装程序并跟踪其进度，同时显示应用的安装进度。 若要启用无提示跟踪，.NET Framework 4.5 安装程序（可观察）通过使用内存映射 I/O (MMIO) 段来定义协议，以便与安装程序（观察程序或链接器）进行通信。 此协议定义链接器获取进度信息、详细结果，响应消息和取消 .NET Framework 4.5 安装的方式。

- **调用**。 若要调用 .NET Framework 4.5 安装程序并接收来自 MMIO 节的进度信息，安装程序必须执行以下操作：

    1. 调用 .NET Framework 4.5 可再发行程序：

        `dotNetFx45_Full_x86_x64.exe /q /norestart /pipe section-name`

        其中“section name”是要用来标识应用的任意名称。 .NET Framework 安装程序以异步方式在 MMIO 节进行读写，因此可能会发现在此期间使用事件和消息很方便。 在示例中，.NET Framework 安装进程由分配 MMIO 节 (`TheSectionName`) 和定义事件 (`TheEventName`) 的构造函数创建：

        ```cpp
        Server():ChainerSample::MmioChainer(L"TheSectionName", L"TheEventName")
        ```

        请使用对于安装程序唯一的名称替换这些名称。

    2. 从 MMIO 节读取。 在 .NET Framework 4.5 中，下载和安装操作是同时进行的：下载 .NET Framework 一部分的同时，可能正在安装另一部分。 因此，进度会以从 0 到 255 递增的两个数字（`m_downloadSoFar` 和 `m_installSoFar`）形式发送回（即写入）MMIO 节。 如果写入 255 且 .NET Framework 存在，则表示安装完成。

- 退出代码。 以下命令中的退出代码用于调用 .NET Framework 4.5 可再发行程序，指示安装是成功还是失败：

  - 0 - 安装已成功完成。

  - 3010 - 安装已成功完成；需要重启系统。

  - 1602 - 安装已取消。

  - 所有其他代码 - 安装过程中出现错误；请检查 %temp% 中创建的日志文件，了解详细信息。

- 取消安装。 可随时通过使用 `Abort` 方法在 MMIO 节中设置 `m_downloadAbort` 和 `m_ installAbort` 标志来取消安装。

## <a name="chainer-sample"></a>链接器示例

链接器示例以无提示方式启动并跟踪 .NET Framework 4.5 安装程序，同时显示进度。 此示例类似于为 .NET Framework 4 提供的链接器示例。 但是，它还可以通过处理用于关闭 .NET framework 4 应用的消息框来避免系统重启。 有关此消息框的信息，请参阅[在 .NET Framework 4.5 安装期间减少系统重启](reducing-system-restarts.md)。 可以将此示例与 .NET Framework 4 安装程序结合使用；在这种情况下，不会发送消息。

> [!WARNING]
> 必须以管理员身份运行此示例。

可从 MSDN 示例库下载针对 [.NET Framework 4.5 链接器示例](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba)的完整 Visual Studio 解决方案。

以下章节对此示例中的重要文件进行了介绍：MMIOChainer.h、ChainingdotNet4.cpp 和 IProgressObserver.h。

#### <a name="mmiochainerh"></a>MMIOChainer.h

- MMIOChainer.h 文件（请参阅[完整代码](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=663039622)）包含数据结构定义和链接器类应派生自的基类。 .NET Framework 4.5 扩展 MMIO 数据结构，处理 .NET Framework 4.5 安装程序需要的数据。 对 MMIO 结构的更改为后向兼容，因此 .NET Framework 4 链接器可在无需重新编译的情况下与 .NET Framework 4.5 安装程序结合使用。 但是，此方案不支持用于减少系统重启的功能。

    版本字段提供标识结构和消息格式的修订的方法。 .NET Framework 安装程序通过调用 `VirtualQuery` 函数来确定文件映射的大小，从而确定链接器接口的版本。 如果大小足以容纳版本字段，则 .NET framework 安装程序将使用该指定值。 如果文件映射因过小而无法包含版本字段（.NET framework 4 即是这种情况），则安装过程假定为版本 0 (4)。 如果链接器不支持 .NET framework 安装程序希望发送的消息版本，则 .NET framework 安装程序会假定一个忽略响应。

    按以下方式定义 MMIO 数据结构：

    ```cpp
    // MMIO data structure for interprocess communication
        struct MmioDataStructure
        {
            bool m_downloadFinished;               // Is download complete?
            bool m_installFinished;                // Is installation complete?
            bool m_downloadAbort;                  // Set to cause downloader to abort.
            bool m_installAbort;                   // Set to cause installer to abort.
            HRESULT m_hrDownloadFinished;          // Resulting HRESULT for download.
            HRESULT m_hrInstallFinished;           // Resulting HRESULT for installation.
            HRESULT m_hrInternalError;
            WCHAR m_szCurrentItemStep[MAX_PATH];
            unsigned char m_downloadSoFar;         // Download progress 0-255 (0-100% done).
            unsigned char m_installSoFar;          // Installation progress 0-255 (0-100% done).
            WCHAR m_szEventName[MAX_PATH];         // Event that chainer creates and chainee opens to sync communications.

            BYTE m_version;                        // Version of the data structure, set by chainer:
                                                   // 0x0: .NET Framework 4
                                                   // 0x1: .NET Framework 4.5

            DWORD m_messageCode;                   // Current message sent by the chainee; 0 if no message is active.
            DWORD m_messageResponse;               // Chainer's response to current message; 0 if not yet handled.
            DWORD m_messageDataLength;             // Length of the m_messageData field, in bytes.
            BYTE m_messageData[1];                 // Variable-length buffer; content depends on m_messageCode.
        };
    ```

- 不应直接使用 `MmioDataStructure` 数据结构；而应改用 `MmioChainer` 类以实现链接器。 派生自 `MmioChainer` 类，链接 .NET Framework 4.5 可再发行组件。

#### <a name="iprogressobserverh"></a>IProgressObserver.h

- IProgressObserver.h 文件实现进度观察器（[请参阅完整代码](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=1263700592)）。 此观察程序会获得下载和安装进度通知（指定为无符号 `char`，0-255，指示 1%-100% 完成进度）。 链接发送消息时，观察程序也会得到通知，并且观察程序应该发送一个答复。

    ```cpp
        class IProgressObserver
        {
        public:
            virtual void OnProgress(unsigned char) = 0; // 0 - 255:  255 == 100%
            virtual void Finished(HRESULT) = 0;         // Called when operation is complete
            virtual DWORD Send(DWORD dwMessage, LPVOID pData, DWORD dwDataLength) = 0; // Called when a message is sent
        };
    ```

#### <a name="chainingdotnet45cpp"></a>ChainingdotNet4.5.cpp

- [ChainingdotNet4.5.cpp](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=1757268882) 文件实现从 `MmioChainer` 派生的 `Server` 类，并重写了相应方法以显示进度信息。 MmioChainer 使用指定的节名称创建一节并使用指定的事件名称初始化链接器。 事件名称会保存在映射数据结构中。 应确保节和事件名称的唯一性。 以下代码中的 `Server` 类会启动指定的安装程序，监视其进度并返回退出代码。

    ```cpp
    class Server : public ChainerSample::MmioChainer, public ChainerSample::IProgressObserver
    {
    public:
        …………….
        Server():ChainerSample::MmioChainer(L"TheSectionName", L"TheEventName") //customize for your event names
        {}
    ```

    在 Main 方法中开始安装。

    ```cpp
    // Main entry point for program
    int __cdecl main(int argc, _In_count_(argc) char **_argv)
    {
        int result = 0;
        CString args;
        if (argc > 1)
        {
            args = CString(_argv[1]);
        }

        if (IsNetFx4Present(NETFX45_RC_REVISION))
        {
            printf(".NET Framework 4.5 is already installed");
        }
        else
        {
            result = Server().Launch(args);
        }

        return result;
    }
    ```

- 启动安装之前，链接器会先进行检查，查看是否已通过调用 `IsNetFx4Present` 安装 .NET Framework 4.5：

    ```cpp
    ///  Checks for presence of the .NET Framework 4.
    ///    A value of 0 for dwMinimumRelease indicates a check for the .NET Framework 4 full
    ///    Any other value indicates a check for a specific compatible release of the .NET Framework 4.
    #define NETFX40_FULL_REVISION 0
    // TODO: Replace with released revision number
    #define NETFX45_RC_REVISION MAKELONG(50309, 5)   // .NET Framework 4.5
    bool IsNetFx4Present(DWORD dwMinimumRelease)
    {
        DWORD dwError = ERROR_SUCCESS;
        HKEY hKey = NULL;
        DWORD dwData = 0;
        DWORD dwType = 0;
        DWORD dwSize = sizeof(dwData);

        dwError = ::RegOpenKeyExW(HKEY_LOCAL_MACHINE, L"SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full", 0, KEY_READ, &hKey);
        if (ERROR_SUCCESS == dwError)
        {
            dwError = ::RegQueryValueExW(hKey, L"Release", 0, &dwType, (LPBYTE)&dwData, &dwSize);

            if ((ERROR_SUCCESS == dwError) && (REG_DWORD != dwType))
            {
                dwError = ERROR_INVALID_DATA;
            }
            else if (ERROR_FILE_NOT_FOUND == dwError)
            {
                // Release value was not found, let's check for 4.0.
                dwError = ::RegQueryValueExW(hKey, L"Install", 0, &dwType, (LPBYTE)&dwData, &dwSize);

                // Install = (REG_DWORD)1;
                if ((ERROR_SUCCESS == dwError) && (REG_DWORD == dwType) && (dwData == 1))
                {
                    // treat 4.0 as Release = 0
                    dwData = 0;
                }
                else
                {
                    dwError = ERROR_INVALID_DATA;
                }
            }
        }

        if (hKey != NULL)
        {
            ::RegCloseKey(hKey);
        }

        return ((ERROR_SUCCESS == dwError) && (dwData >= dwMinimumRelease));
    }
    ```

- 可以更改 `Launch` 方法中可执行组件（在此示例中为 Setup.exe）的路径以指向其正确位置，或者自定义代码以确定位置。 `MmioChainer` 基类提供派生类调用的阻止 `Run()` 方法。

    ```cpp
    bool Launch(const CString& args)
    {
    CString cmdline = L"dotNetFx45_Full_x86_x64.exe -pipe TheSectionName " + args; // Customize with name and location of setup .exe that you want to run
    STARTUPINFO si = {0};
    si.cb = sizeof(si);
    PROCESS_INFORMATION pi = {0};

    // Launch the Setup.exe that installs the .NET Framework 4.5
    BOOL bLaunchedSetup = ::CreateProcess(NULL,
     cmdline.GetBuffer(),
     NULL, NULL, FALSE, 0, NULL, NULL,
     &si,
     &pi);

    // If successful
    if (bLaunchedSetup != 0)
    {
    IProgressObserver& observer = dynamic_cast<IProgressObserver&>(*this);
    Run(pi.hProcess, observer);

    ……………………..
    return (bLaunchedSetup != 0);
    }
    ```

- `Send` 方法截获并处理消息。 在此版本的 .NET Framework 中，唯一支持的消息是关闭应用程序消息。

    ```cpp
            // SendMessage
            //
            // Send a message and wait for the response.
            // dwMessage: Message to send
            // pData: The buffer to copy the data to
            // dwDataLength: Initially a pointer to the size of pBuffer. Upon successful call, the number of bytes copied to pBuffer.
            //--------------------------------------------------------------
        virtual DWORD Send(DWORD dwMessage, LPVOID pData, DWORD dwDataLength)
        {
            DWORD dwResult = 0;
            printf("received message: %d\n", dwMessage);
            // Handle message
            switch (dwMessage)
            {
            case MMIO_CLOSE_APPS:
                {
                    printf("    applications are holding files in use:\n");
                    IronMan::MmioCloseApplications* applications = reinterpret_cast<IronMan::MmioCloseApplications*>(pData);
                    for(DWORD i = 0; i < applications->m_dwApplicationsSize; i++)
                    {
                        printf("      %ls (%d)\n", applications->m_applications[i].m_szName, applications->m_applications[i].m_dwPid);
                    }

                    printf("    should appliations be closed? (Y)es, (N)o, (R)efresh : ");
                    while (dwResult == 0)
                    {
                        switch (toupper(getwchar()))
                        {
                        case 'Y':
                            dwResult = IDYES;  // Close apps
                            break;
                        case 'N':
                            dwResult = IDNO;
                            break;
                        case 'R':
                            dwResult = IDRETRY;
                            break;
                        }
                    }
                    printf("\n");
                    break;
                }
            default:
                break;
            }
            printf("  response: %d\n  ", dwResult);
            return dwResult;
        }
    };
    ```

- 进度数据是 0 (0%) 到 255 (100%) 之间的无符号 `char`。

    ```cpp
    private: // IProgressObserver
        virtual void OnProgress(unsigned char ubProgressSoFar)
        {…………
       }
    ```

- 将 HRESULT 传递给 `Finished` 方法。

    ```cpp
    virtual void Finished(HRESULT hr)
    {
    // This HRESULT is communicated over MMIO and may be different than process
    // Exit code of the Chainee Setup.exe itself
    printf("\r\nFinished HRESULT: 0x%08X\r\n", hr);
    }
    ```

    > [!IMPORTANT]
    > .NET Framework 4.5 可再发行组件通常会写入多条进度消息和一条指示完成的消息（在链接器端）。 它还会以异步方式进行读取，查找 `Abort` 记录。 如果接收到 `Abort` 记录，则会取消安装，并在安装中止和安装操作回退后，使用 E ABORT 作为其数据编写已完成记录。

典型的服务器会创建随机 MMIO 文件名、创建文件（如上一个代码示例 `Server::CreateSection` 中所示），并通过使用 `CreateProcess` 和借助 `-pipe someFileSectionName` 选项传递管道名称来启动可再发行组件。 服务器应使用特定于应用程序 UI 的代码来实现 `OnProgress`、`Send` 和 `Finished` 方法。

## <a name="see-also"></a>请参阅

- [面向开发人员的部署指南](deployment-guide-for-developers.md)
- [部署](index.md)
