---
ms.openlocfilehash: 735278848cb7399e414a128afc650a0a1f882337
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "67857492"
---
### <a name="changing-the-isenabled-property-of-the-parent-of-a-textblock-control-affects-any-child-controls"></a>變更 TextBlock 控制項之父代的 IsEnabled 屬性會影響任何子控制項

|   |   |
|---|---|
|詳細資料|從 .NET Framework 4.6.2 開始，變更 <xref:System.Windows.Controls.TextBlock?displayProperty=name> 控制項之父代的 <xref:System.Windows.UIElement.IsEnabled?displayProperty=name> 屬性，會影響 <xref:System.Windows.Controls.TextBlock?displayProperty=name> 控制項的任何子控制項 (例如超連結和按鈕)。在 .NET Framework 4.6.1 和舊版中，<xref:System.Windows.Controls.TextBlock?displayProperty=name> 內的控制項不一定會反映 <xref:System.Windows.Controls.TextBlock?displayProperty=name> 父代的 <xref:System.Windows.UIElement.IsEnabled?displayProperty=name> 屬性狀態。|
|建議|無。 這項變更符合 <xref:System.Windows.Controls.TextBlock?displayProperty=name> 控制項內的之控制項的預期行為。|
|影響範圍|Minor|
|版本|4.6.2|
|類型|執行階段|
|受影響的 API|<ul><li><xref:System.Windows.UIElement.IsEnabled?displayProperty=nameWithType></li></ul>|
