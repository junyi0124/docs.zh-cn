---
title: <permission>
ms.date: 07/20/2015
helpviewer_keywords:
- <permission> XML tag
- permission XML tag
ms.assetid: 0edf0500-5cd7-49c0-9255-64c48f972b77
ms.openlocfilehash: ae6167f3582fe22cd10d9ef7a10873d6d9bdfa06
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872544"
---
# <a name="permission-visual-basic"></a>\<permission> (Visual Basic)

指定成员所需的权限。  
  
## <a name="syntax"></a>语法  
  
```xml  
<permission cref="member">description</permission>  
```  
  
## <a name="parameters"></a>参数  

 `member`  
 对可从当前编译环境调用的成员或字段的引用。 编译器检查是否存在给定的代码元素，并将 `member` 转换为输出 XML 中规范的元素名称。 用 `member` 引号将 ( "" ) 。  
  
 `description`  
 对成员访问权限的说明。  
  
## <a name="remarks"></a>备注  

 使用 `<permission>` 标记来记录成员的访问。 使用 <xref:System.Security.PermissionSet> 类指定对成员的访问权限。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<permission>` 标记来描述 <xref:System.Security.Permissions.FileIOPermission> 方法所需的 `ReadFile` 。  
  
 [!code-vb[VbVbcnXmlDocComments#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#7)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
