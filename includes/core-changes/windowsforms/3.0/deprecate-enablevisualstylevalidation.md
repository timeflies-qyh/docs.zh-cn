---
ms.openlocfilehash: 75baa4f23eae838defafd3ce9b3907a187982a18
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75936988"
---
### <a name="enablevisualstylevalidation-compatibility-switch-not-supported"></a>不支持 EnableVisualStyleValidation 兼容性开关

.NET Core 3.0 上的 Windows 窗体不支持 `Switch.System.Windows.Forms.EnableVisualStyleValidation` 兼容性开关。

#### <a name="change-description"></a>更改描述

在 .NET Framework 中，`Switch.System.Windows.Forms.EnableVisualStyleValidation` 兼容性开关使得应用程序可选择不验证以数值格式提供的视觉样式。

.NET Core 中尚不支持 `Switch.System.Windows.Forms.EnableVisualStyleValidation` 开关。

#### <a name="version-introduced"></a>引入的版本

3.0 预览版 9

#### <a name="recommended-action"></a>建议操作

删除此开关。 此开关不受支持，且未提供替代功能。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

- None

<!-- 

### Affected APIs

- Not detectable via API analysis

-->
