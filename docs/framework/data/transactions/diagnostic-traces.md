---
title: 診斷追蹤
ms.date: 03/30/2017
ms.assetid: 28e77a63-d20d-4b6a-9caf-ddad86550427
ms.openlocfilehash: 76712710bf42f498ba859c7b1cd18a261387078c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79174416"
---
# <a name="diagnostic-traces"></a>診斷追蹤
追蹤就是在應用程式執行期間，所產生的特定訊息之發行動作。 使用追蹤功能時，您必須具有收集和記錄所傳送訊息的機制。 追蹤訊息由「接聽項」負責接收。 接聽項的用途是收集、儲存和傳送追蹤訊息。 接聽項會將追蹤輸出導向至適當的目標，例如記錄檔、視窗或文字檔。  
  
 這類的接聽項，例如 <xref:System.Diagnostics.DefaultTraceListener>，會在啟用追蹤時自訂建立與初始。 如果您要將追蹤輸出傳送至任何其他來源，您必須建立和初始化其他的追蹤接聽項。 您建立的接聽項應反映出您個人的需求。 例如，您可能想要所有追蹤輸出的文字記錄。 在這種情況中，您建立的接聽項要在啟用時，將所有的輸出寫入新的文字檔。 另一方面，您可能只想在應用程式執行期間檢視輸出。 在這種情況中，您可建立導向所有輸出至主控台視窗的接聽項。 <xref:System.Diagnostics.EventLogTraceListener> 可以導向追蹤輸出至事件記錄檔，而 <xref:System.Diagnostics.TextWriterTraceListener> 可將追蹤輸出寫入資料流。  
  
## <a name="enabling-tracing"></a>啟用追蹤  
 若要在交易處理期間啟用追蹤，您應該編輯應用程式的組態檔。 以下是一個範例。  
  
```xml  
<configuration>  
<system.diagnostics>  
     <sources>  
          <source name="System.Transactions" switchValue="Warning">  
               <listeners>  
                    <add name="tx"
                     type="System.Diagnostics.XmlWriterTraceListener"
                     initializeData= "tx.log" />  
               </listeners>  
          </source>  
     </sources>  
</system.diagnostics>  
</configuration>  
```  
  
 <xref:System.Transactions> 追蹤會寫入名為 "System.Transactions" 的來源中。 您可以使用 `add` 來指定要使用的追蹤接聽項名稱與型別。 在我們的組態範例中，我們將接聽項命名為 "tx" 並將標準 .NET Framework 追蹤接聽項 (<xref:System.Diagnostics.XmlWriterTraceListener>) 當成要使用的型別加入。 請使用 `initializeData` 來設定該接聽項的記錄檔名稱。 此外，您可以使用完整路徑來取代簡單檔名。  
  
 每個追蹤訊息類型都會指派一個層級來代表自己的重要程度。 如果應用程式定義域的追蹤層級等於或小於事件型別的層級，則會產生該訊息。 追蹤層級可由組態檔中的 `switchValue` 設定來控制。 下表定義了與診斷追蹤訊息相關聯的層級。  
  
|追蹤層級|描述|  
|-----------------|-----------------|  
|重大|已發生如下列所述的嚴重失敗情況：<br /><br /> - 可能導致使用者功能立即丟失的錯誤。<br />- 需要管理員採取措施以避免功能遺失的事件。<br />- 代碼掛起。<br />- 此跟蹤級別還可以為解釋其他關鍵跟蹤提供足夠的上下文。 如此便可協助找出導致嚴重失敗的作業序列。|  
|錯誤|已經發生可導致使用者功能喪失的錯誤 (例如，無效的組態或網路行為)。|  
|警告|存在一個可間接導致錯誤或嚴重失敗的狀況 (例如，配置失敗或到達上限)。 對使用者程式碼錯誤的正常處理 (例如，交易中止、逾時、驗證失敗) 也可能產生警告。|  
|資訊|會產生對監控與診斷系統狀態、衡量效能，或描述分析有幫助的訊息。 這些訊息包含交易與登記存留期事件，例如正在建立或認可的交易、重要界限的跨越，或是重要資源的配置。 這時開發人員可以使用此類資訊來進行容量規劃與效能管理。|  
  
## <a name="trace-codes"></a>追蹤程式碼  
 下表列出由 <xref:System.Transactions> 基礎結構所產生的追蹤程式碼。 表中包括跟蹤代碼識別碼、跟蹤的枚<xref:System.Diagnostics.EventTypeFilter.EventType%2A>舉級別以及跟蹤的**TraceRecord**中包含的額外資料。 此外，跟蹤的相應跟蹤級別也存儲在**追蹤記錄中**。  
  
|TraceCode|EventType|TraceRecord 中的額外資料|  
|---------------|---------------|-------------------------------|  
|TransactionCreated|資訊|TransactionTraceId|  
|TransactionPromoted|資訊|本機 TransactionTraceId、Distributed TransactionTraceId|  
|EnlistmentCreated|資訊|TransactionTraceId、EnlistmentTraceId、EnlistmentType (永久性/變動性)、EnlistmentOptions|  
|EnlistmentCallbackNegative|警告|TransactionTraceId、EnlistmentTraceId、<br /><br /> 回呼 (forcerollback/中止/indoubt)|  
|TransactionRollbackCalled|警告|TransactionTraceId|  
|TransactionAborted|警告|TransactionTraceId|  
|TransactionInDoubt|警告|TransactionTraceId|  
|TransactionScopeCreated|資訊|TransactionScopeResult，可能為下列各項：<br /><br /> - 新交易<br />- 交易通過。<br />- 已傳遞從屬事務。<br />- 使用當前事務。<br />-沒有交易<br /><br /> 目前新 TransactionTraceId|  
|TransactionScopeDisposed|資訊|作用域的"預期"當前事務的事務跟蹤Id。|  
|TransactionScopeIncomplete|警告|作用域的"預期"當前事務的事務跟蹤Id。|  
|TransactionScopeNestedIncorrectly|警告|作用域的"預期"當前事務的事務跟蹤Id。|  
|TransactionScopeCurrentTransactionChanged|警告|目前舊有 TransactionTraceId、其他 TransactionTraceId|  
|TransactionScopeTimeout|警告|作用域的"預期"當前事務的事務跟蹤Id。|  
|DependentCloneCreated|資訊|TransactionTraceId、所建立的相依交易型別 (RollbackIfNotComplete/BlockCommitUntilComplete)|  
|DependentCloneComplete|資訊|TransactionTraceId|  
|RecoveryComplete|資訊|資源管理員 GUID (從基底)|  
|Reenlist|資訊|資源管理員 GUID (從基底)|  
|TransactionSerialized|資訊|TransactionTraceId。|  
|TransactionException|錯誤|例外狀況訊息|  
|InvalidOperationException|錯誤|例外狀況訊息|  
|InternalError|重大|例外狀況訊息|  
|TransferEvent||當交易已還原序列化，或是由 <xref:System.Transactions> 交易提升為分散式交易，則會寫入來自 ExecutionContext 與分散式交易識別碼的目前 ActivityID。<br /><br /> 當 DTC 回呼 Managed 程式碼，分散式交易識別碼就會針對回呼的持續時間設為 ExecutionContext 中的 ActivityID。|  
|ConfiguredDefaultTimeoutAdjusted|警告|無額外的資料|  
|TransactionTimeout|警告|目前逾時的交易 TransactionTraceId。|  
  
 每個前置額外資料項目的 XML 結構描述都具有下列格式。  
  
### <a name="transactiontraceidentifier"></a>TransactionTraceIdentifier  
 `<TransactionTraceIdentifier>`  
  
 `<TransactionIdentifier >`  
  
 `string representation of transaction id`  
  
 `</TransactionIdentifier>`  
  
 `< CloneIdentifier >`  
  
 `the clone id number`  
  
 `</CloneIdentifier>`  
  
 `</TransactionTraceIdentifier>`  
  
### <a name="enlistmenttraceidentifier"></a>EnlistmentTraceIdentifier  
 `<EnlistmentTraceIdentifier>`  
  
 `<ResourceManagerId>`  
  
 `string form of guid`  
  
 `</ResourceManagerId>`  
  
 `<TransactionTraceIdentifier>`  
  
 `<TransactionIdentifier >`  
  
 `string representation of transaction id`  
  
 `</TransactionIdentifier>`  
  
 `<CloneIdentifier >`  
  
 `the clone id number`  
  
 `</CloneIdentifier>`  
  
 `<TransactionTraceIdentifier>`  
  
 `<EnlistmentIdentifier>`  
  
 `the enlistment id number`  
  
 `</EnlistmentIdentifier>`  
  
 `</EnlistmentTraceIdentifier>`  
  
### <a name="resource-manager-identifier"></a>資源管理員識別項  
 `<ResourceManagerId>`  
  
 `string form of guid`  
  
 `</ResourceManagerId>`  
  
## <a name="security-issues-for-tracing"></a>追蹤安全性問題  
 當您以管理員身分開啟追蹤功能時，敏感性資訊可能會寫入預設可公開檢視的追蹤記錄中。 為了緩和任何可能發生的安全性威脅，您應該考慮將追蹤記錄存放在由共用與檔案系統存取權限所控制的安全地點。
