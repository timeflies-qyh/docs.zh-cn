---
title: Windows 窗体中断性变更 - .NET Core
description: 列出适用于 .NET Core 的 Windows 窗体中的中断性变更。
ms.date: 01/08/2020
ms.openlocfilehash: 44bcde60f9e08d2e06a69c55e4ebe904bf5c449b
ms.sourcegitcommit: ed3f926b6cdd372037bbcc214dc8f08a70366390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76116453"
---
# <a name="breaking-changes-in-windows-forms"></a>Windows 窗体中的中断性变更

已在 .NET Core 的版本 3.0 中添加 Windows 窗体支持。 本文按引入中断性变更的 .NET Core 版本列出了 Windows 窗体的中断性变更。 如果要从 .NET Framework 或 .NET Core 的早期版本（3.0 或更高版本）升级 Windows 窗体应用，本文能为你提供帮助。

本页记录了以下中断性变更：

- [已删除的控件](#removed-controls)
- [如果显示工具提示，则不引发 CellFormatting 事件](#cellformatting-event-not-raised-if-tooltip-is-shown)
- [Control.DefaultFont 已更改为 Segoe UI 9 pt](#default-control-font-changed-to-segoe-ui-9-pt)
- [FolderBrowserDialog 的现代化](#modernization-of-the-folderbrowserdialog)
- [已从一些 Windows 窗体类型中删除 SerializableAttribute](#serializableattribute-removed-from-some-windows-forms-types)
- [不支持 AllowUpdateChildControlIndexForTabControls 兼容性开关](#allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported)
- [不支持 DomainUpDown.UseLegacyScrolling 兼容性开关](#domainupdownuselegacyscrolling-compatibility-switch-not-supported)
- [不支持 DoNotLoadLatestRichEditControl 兼容性开关](#donotloadlatestricheditcontrol-compatibility-switch-not-supported)
- [不支持 DoNotSupportSelectAllShortcutInMultilineTextBox 兼容性开关](#donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported)
- [不支持 DontSupportReentrantFilterMessage 兼容性开关](#dontsupportreentrantfiltermessage-compatibility-switch-not-supported)
- [不支持 EnableVisualStyleValidation 兼容性开关](#enablevisualstylevalidation-compatibility-switch-not-supported)
- [不支持 UseLegacyContextMenuStripSourceControlValue 兼容性开关](#uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported)
- [不支持 UseLegacyImages 兼容性开关](#uselegacyimages-compatibility-switch-not-supported)
- [更改 AccessibleObject.RuntimeIDFirstItem 的访问权限](#change-of-access-for-accessibleobjectruntimeidfirstitem)
- [已 Windows 窗体中删除重复的 API](#duplicated-apis-removed-from-windows-forms)

## <a name="net-core-31"></a>.NET Core 3.1

[!INCLUDE[Removed controls](~/includes/core-changes/windowsforms/3.1/remove-controls-3.1.md)]

***

[!INCLUDE[CellFormatting event](~/includes/core-changes/windowsforms/3.1/cellformatting-event-not-raised.md)]

***

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Control.DefaultFont changed to Segoe UI 9pt](~/includes/core-changes/windowsforms/3.0/control-defaultfont-changed.md)]

***

[!INCLUDE[Modernization of the FolderBrowserDialog](~/includes/core-changes/windowsforms/3.0/modernized-folderbrowserdialog.md)]

***

[!INCLUDE[SerializableAttribute removed from some Windows Forms types](~/includes/core-changes/windowsforms/3.0/remove-serializationattribute.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-allowupdatechildcontrolindexfortabcontrols.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyscrolling.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DoNotLoadLatestRichEditControl compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotloadlatestricheditcontrol.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotsupportselectallshortcutinmultilinetextbox.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DontSupportReentrantFilterMessage compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-dontsupportreentrantfiltermessage.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.EnableVisualStyleValidation compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-enablevisualstylevalidation.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacycontextmenustripsourcecontrolvalue.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.UseLegacyImages compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyimages.md)]

***

[!INCLUDE[Change of access for AccessibleObject.RuntimeIDFirstItem](~/includes/core-changes/windowsforms/3.0/changed-access-for-runtimeidfirstitem.md)]

***

[!INCLUDE[Duplicated APIs removed from Windows Forms](~/includes/core-changes/windowsforms/3.0/remove-duplicated-apis.md)]

***

## <a name="see-also"></a>请参阅

- [将 Windows 窗体应用移植到 .NET Core](../porting/winforms.md)
