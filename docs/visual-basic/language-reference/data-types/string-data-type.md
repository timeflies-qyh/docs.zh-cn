---
title: String 数据类型
ms.date: 07/20/2015
f1_keywords:
- vb.String
helpviewer_keywords:
- strings [Visual Basic], character
- strings [Visual Basic], fixed-length
- string keyword [Visual Basic]
- fixed-length strings [Visual Basic], string data type
- literals [Visual Basic], string
- text [Visual Basic], String data type
- $ identifier type character
- String data type
- fixed-length strings
- string literals [Visual Basic]
- data types [Visual Basic], assigning
- String literals [Visual Basic]
- identifier type characters [Visual Basic], $
ms.assetid: 15ac03f5-cabd-42cc-a754-1df3893c25d9
ms.openlocfilehash: c2c6f9632646c432abb7b6da8887253e526cc994
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343912"
---
# <a name="string-data-type-visual-basic"></a>String 数据类型 (Visual Basic)

保存范围介于0到65535之间的16位无符号16位（2字节）码位序列。 每个*码位*或字符代码都表示一个 Unicode 字符。 字符串可以包含0到约2000000000（2 ^ 31）个 Unicode 字符。  
  
## <a name="remarks"></a>备注  

 使用 `String` 数据类型来存储多个字符，而无需 `Char()`的数组管理开销，`Char` 元素的数组。  
  
 `String` 的默认值为 `Nothing` （空引用）。 请注意，这与空字符串（值 `""`）不同。  
  
## <a name="unicode-characters"></a>Unicode 字符  

 Unicode 的第一个128码位（0–127）对应于标准美式键盘上的字母和符号。 这前128码位与 ASCII 字符集定义的代码点相同。 第二个128码位（128–255）表示特殊字符，例如基于拉丁语的字母表号、重音、货币符号和分数。 Unicode 对各种符号使用剩余的代码点（256-65535）。 这包括全球文本字符、音调符号、数学和技术符号。  
  
 您可以使用方法（如 <xref:System.Char.IsDigit%2A>）和 <xref:System.Char.IsPunctuation%2A> `String` 变量中的单个字符来确定其 Unicode 分类。  
  
## <a name="format-requirements"></a>格式要求  

 必须将 `String` 文本用引号引起来（`" "`）。 如果必须包括引号作为字符串中的字符之一，请使用两个连续的引号（`""`）。 下面的示例对此进行了演示。  
  
```vb  
Dim j As String = "Joe said ""Hello"" to me."  
Dim h As String = "Hello"  
' The following messages all display the same thing:  
' "Joe said "Hello" to me."  
MsgBox(j)  
MsgBox("Joe said " & """" & h & """" & " to me.")  
MsgBox("Joe said """ & h & """ to me.")  
```  
  
 请注意，表示字符串中的引号的连续引号与开始和结束 `String` 文本的引号无关。  
  
## <a name="string-manipulations"></a>字符串操作  

 将字符串分配给 `String` 变量后，该字符串是*不可变*的，这意味着你无法更改其长度或内容。 以任何方式更改字符串时，Visual Basic 会创建一个新字符串，并放弃上一个字符串。 然后，`String` 变量指向新的字符串。  
  
 您可以使用各种字符串函数来处理 `String` 变量的内容。 下面的示例说明 <xref:Microsoft.VisualBasic.Strings.Left%2A> 函数  
  
```vb  
Dim S As String = "Database"  
' The following statement sets S to a new string containing "Data".  
S = Microsoft.VisualBasic.Left(S, 4)  
```  
  
 其他组件创建的字符串可能用前导空格或尾随空格填充。 如果收到这样的字符串，可以使用 <xref:Microsoft.VisualBasic.Strings.Trim%2A>、<xref:Microsoft.VisualBasic.Strings.LTrim%2A>和 <xref:Microsoft.VisualBasic.Strings.RTrim%2A> 函数删除这些空格。  
  
 有关字符串操作的详细信息，请参阅[字符串](../../../visual-basic/programming-guide/language-features/strings/index.md)。  
  
## <a name="programming-tips"></a>编程提示  
  
- **负数。** 请记住，`String` 保留的字符是无符号的，不能表示负值。 在任何情况下，不应使用 `String` 来保存数值。  
  
- **互操作注意事项。** 如果你与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）进行交互，请记住，在其他环境中，字符串字符具有不同的数据宽度（8位）。 如果要将8位字符的字符串参数传递给此类组件，则将其声明为 `Byte()`、`Byte` 元素的数组，而不是在新的 Visual Basic 代码中 `String`。  
  
- **键入字符。** 如果将标识符类型字符 `$` 追加到任何标识符，则会将其强制转换为 `String` 的数据类型。 `String` 没有文本类型字符。 但编译器会将括在引号（`" "`）中的文本视为 `String`。  
  
- **Framework 类型。** .NET Framework 中的相应类型是 <xref:System.String?displayProperty=nameWithType> 类。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.String?displayProperty=nameWithType>
- [数据类型](../../../visual-basic/language-reference/data-types/index.md)
- [Char 数据类型](../../../visual-basic/language-reference/data-types/char-data-type.md)
- [Type Conversion Functions](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [转换摘要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [如何：调用采用无符号类型的 Windows 函数](../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [有效使用数据类型](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
