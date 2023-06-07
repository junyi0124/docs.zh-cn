---
title: Cert2spc.exe（软件发行者证书测试工具）
description: 使用 Cert2spc.exe（软件发行者证书测试工具）。 此工具通过一个或多个 X.509 证书创建发行者证书 (SPC)。
ms.date: 03/30/2017
helpviewer_keywords:
- SPC
- Software Publisher Certificate Test tool
- Software Publisher Certificate
- Cert2spc.exe
- certificates, Software Publisher's Certificate
ms.assetid: be434d7d-9c0d-46e7-8392-58a9b542d11d
ms.openlocfilehash: 0bcc785a51f2ca46195970130178d0cfa705ee6e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247317"
---
# <a name="cert2spcexe-software-publisher-certificate-test-tool"></a>Cert2spc.exe（软件发行者证书测试工具）

软件发行者证书测试工具从一个或多个 X.509 证书创建软件发行者证书 (SPC)。 Cert2spc.exe 仅用于测试目的。 可以从证书颁发机构（如 VeriSign 或 Thawte）获得有效 SPC。 有关创建 X.509 证书的详细信息，请参阅 [Makecert.exe（证书创建工具）](/windows/desktop/SecCrypto/makecert)。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行此工具，请使用 Visual Studio 开发人员命令提示（或 Windows 7 中的 Visual Studio 命令提示）。 有关详细信息，请参阅[命令提示](developer-command-prompt-for-vs.md)。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
cert2spc cert1.cer | crl1.crl [... certN.cer | crlN.crl] outputSPCfile.spc  
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|--------------|-----------------|  
|`certN.cer`|要包含在 SPC 文件中的 X.509 证书的名称。 可以指定用空格分隔的多个名称。|  
|`crlN.crl`|要包含在 SPC 文件中的证书吊销列表的名称。 可以指定用空格分隔的多个名称。|  
|`outputSPCfile.spc`|将包含 X.509 证书的 PKCS #7 对象的名称。|  
  
|选项|说明|  
|------------|-----------------|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="examples"></a>示例  

 下面的命令从 `myCertificate.cer` 创建一个 SPC 并将其放入 `mySPCFile.spc`。  
  
```console
cert2spc myCertificate.cer mySPCFile.spc  
```  
  
 下面的命令从 `oneCertificate.cer` 和 `twoCertificate.cer` 创建一个 SPC，并将其放入 `mySPCFile.spc`。  
  
```console
cert2spc oneCertificate.cer twoCertificate.cer mySPCFile.spc  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Makecert.exe（证书创建工具）](/windows/desktop/SecCrypto/makecert)
- [命令提示](developer-command-prompt-for-vs.md)
