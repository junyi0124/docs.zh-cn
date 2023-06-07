---
title: 针对共享 Web 承载优化
ms.date: 03/30/2017
helpviewer_keywords:
- garbage collection, Web hosting
- garbage collection, optimizing
- garbage collection, shared Web hosting
ms.assetid: be98c0ab-7ef8-409f-8a0d-cb6e5b75ff20
ms.openlocfilehash: abab4ac451a70fbc81ac6d7c9da6f8d0123287a8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669024"
---
# <a name="optimization-for-shared-web-hosting"></a>针对共享 Web 承载优化

如果是通过托管多个小型网站进行共享的服务器的管理员，可以将下列 `gcTrimCommitOnLowMemory` 设置添加到 .NET 目录中 Aspnet.config 文件内的 `runtime` 节点，从而优化性能和增加网站容量：  
  
 `<gcTrimCommitOnLowMemory enabled="true|false"/>`  
  
> [!NOTE]
> 此建议设置仅适用于共享 Web 托管方案。  
  
 由于垃圾回收器保留内存以供将来分配，因此它提交的空间可能会超过真正所需。 可以减少此空间来适应系统内存负载过重的情况。 减少提交的此空间可提升性能，并将容量扩展为托管更多网站。  
  
 如果启用 `gcTrimCommitOnLowMemory` 设置，垃圾回收器会计算系统内存负载，并在负载达到 90% 时进入修整模式。 除非负载下降到不到 85%，否则会一直处于修整模式。  
  
 如果条件允许，垃圾回收器可以决定 `gcTrimCommitOnLowMemory` 设置对当前应用没有帮助并忽略它。  
  
## <a name="example"></a>示例  

 下面的 XML 片段展示了如何启用 `gcTrimCommitOnLowMemory` 设置。 省略号表示 `runtime` 节点中会有其他设置。  
  
```xml  
<?xml version="1.0" encoding="UTF-8"?>  
<configuration>  
    <runtime>  
    . . .  
    <gcTrimCommitOnLowMemory enabled="true"/>  
    </runtime>  
    . . .  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [垃圾回收](index.md)
