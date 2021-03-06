---
ms.openlocfilehash: 71c81cf188fa4c2300661f10eb87e7ae00e031f6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804514"
---
### <a name="etw-event-names-cannot-differ-only-by-a-start-or-stop-suffix"></a>ETW 事件名稱不能只有 "Start" 或 "Stop" 尾碼不同

|   |   |
|---|---|
|詳細資料|在 .NET Framework 4.6 和 4.6.1 中，當兩個 Windows 事件追蹤 (ETW) 事件名稱的差異只在 &quot;Start&quot; 或 &quot;Stop&quot; 尾碼時 (例如當一個事件的名稱為 <code>LogUser</code> 而另一個事件的名稱為 <code>LogUserStart</code> 時)，執行階段會擲回 <xref:System.ArgumentException>。 在此情況下，執行階段無法建構事件來源，因此無法發出任何記錄。|
|建議|若要避免這個例外狀況，請確定沒有兩個事件的名稱差異只在 &quot;Start&quot; 或 &quot;Stop&quot; 尾碼。從 .NET Framework 4.6.2 開始已移除這項需求；執行階段可以釐清差異只在於 &quot;Start&quot; 和 &quot;Stop&quot; 尾碼的事件名稱。|
|影響範圍|Edge|
|版本|4.6|
|類型|正在重定目標|
