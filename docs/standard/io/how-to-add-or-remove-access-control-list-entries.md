---
title: 如何：添加或删除访问控制列表条目（仅限 .NET Framework）
ms.date: 01/14/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- ACEs [.NET Framework]
- ACLs [.NET Framework]
- access control entries [.NET Framework]
- I/O [.NET Framework], access control list entries
- access control lists [.NET Framework]
ms.assetid: 53758b39-bd9b-4640-bb04-cad5ed8d0abf
ms.openlocfilehash: 53daa88b761f46dab26b1c12c73741e880512d8d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95682687"
---
# <a name="how-to-add-or-remove-access-control-list-entries-net-framework-only"></a>如何：添加或删除访问控制列表条目（仅限 .NET Framework）

若要向文件或目录添加或从文件或目录删除访问控制列表 (ACL) 条目，请从文件或目录获取 <xref:System.Security.AccessControl.FileSecurity> 或 <xref:System.Security.AccessControl.DirectorySecurity> 对象。 修改对象，然后将其应用回文件或目录。  
  
## <a name="add-or-remove-an-acl-entry-from-a-file"></a>添加或从文件中删除 ACL 条目  
  
1. 调用 <xref:System.IO.File.GetAccessControl%2A?displayProperty=nameWithType> 方法以获取 <xref:System.Security.AccessControl.FileSecurity> 对象，该对象包含文件的当前 ACL 条目。  
  
2. 添加或从步骤 1 返回的 <xref:System.Security.AccessControl.FileSecurity> 对象中删除 ACL 条目。  
  
3. 将 <xref:System.Security.AccessControl.FileSecurity> 对象传递给 <xref:System.IO.File.SetAccessControl%2A?displayProperty=nameWithType> 方法以应用更改。  
  
## <a name="add-or-remove-an-acl-entry-from-a-directory"></a>添加或从目录中删除 ACL 条目  
  
1. 调用 <xref:System.IO.Directory.GetAccessControl%2A?displayProperty=nameWithType> 方法以获取 <xref:System.Security.AccessControl.DirectorySecurity> 对象，该对象包含目录的当前 ACL 条目。  
  
2. 添加或从步骤 1 返回的 <xref:System.Security.AccessControl.DirectorySecurity> 对象中删除 ACL 条目。  
  
3. 将 <xref:System.Security.AccessControl.DirectorySecurity> 对象传递给 <xref:System.IO.Directory.SetAccessControl%2A?displayProperty=nameWithType> 方法以应用更改。  
  
## <a name="example"></a>示例  

 你必须使用有效的用户或组帐户以运行此示例。 此示例使用 <xref:System.IO.File> 对象。 对 <xref:System.IO.FileInfo>、<xref:System.IO.Directory> 和 <xref:System.IO.DirectoryInfo> 类使用相同的过程。

 [!code-csharp[IO.File.GetAccessControl-SetAccessControl#1](../../../samples/snippets/csharp/VS_Snippets_CLR/IO.File.GetAccessControl-SetAccessControl/CS/sample.cs#1)]
 [!code-vb[IO.File.GetAccessControl-SetAccessControl#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/IO.File.GetAccessControl-SetAccessControl/VB/sample.vb#1)]  
