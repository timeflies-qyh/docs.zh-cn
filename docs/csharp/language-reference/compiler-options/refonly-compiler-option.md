---
title: -refonly（C# 编译器选项）
ms.date: 07/08/2017
f1_keywords:
- /refonly
helpviewer_keywords:
- /refonly compiler option [C#]
- -refonly compiler option [C#]
- refonly compiler option [C#]
ms.openlocfilehash: 856b65d3b2217dbe5d53ecda00723b47247d80a4
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72773855"
---
# <a name="-refonly-c-compiler-options"></a>-refonly（C# 编译器选项）

-refonly 选项表示应输出引用程序集（而不是实现程序集）作为主输出  。 `-refonly` 参数以无提示方式禁用输出 PDB，因为无法执行引用程序集。 此选项对应于 MSBuild 的 [ProduceOnlyReferenceAssembly](/visualstudio/msbuild/common-msbuild-project-properties) 项目属性。

## <a name="syntax"></a>语法

```console
-refonly
```

## <a name="remarks"></a>备注

引用程序集是一种特殊类型的程序集，它只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。 有关详细信息，请参阅 .NET 指南中的[引用程序集](../../../standard/assembly/reference-assemblies.md)。

`-refonly` 和 [`-refout`](refout-compiler-option.md) 选项互斥。

## <a name="see-also"></a>请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
