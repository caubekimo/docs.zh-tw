---
ms.openlocfilehash: 8db115a46df3fcea103e8fa6896542d0116aa256
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804658"
---
### <a name="vbnet-no-longer-supports-partial-namespace-qualification-for-systemwindows-apis"></a>VB.NET 不再支援 System.Windows API 的部分命名空間限定性條件

|   |   |
|---|---|
|詳細資料|從 .NET Framework 4.5.2 開始，VB.NET 專案無法以部分限定的命名空間來指定 System.Windows API。 例如，參考 <code>Windows.Forms.DialogResult</code> 將會失敗。 相反地，程式碼必須參考完整名稱 (<xref:System.Windows.Forms.DialogResult>)，或匯入特定命名空間並只參考 <xref:System.Windows.Forms.DialogResult?displayProperty=name>。|
|建議|您應該更新程式碼，以簡單名稱 (並匯入相關命名空間) 或完整名稱參考 <code>System.Windows</code> API。|
|影響範圍|Minor|
|版本|4.5.2|
|類型|正在重定目標|
