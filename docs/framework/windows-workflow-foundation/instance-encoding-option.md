---
title: 執行個體編碼選項
ms.date: 03/30/2017
ms.assetid: 89e4b029-4f68-438c-8117-9b21fe094ef4
ms.openlocfilehash: c4de7c45d899f45a7b5b71d563257d9accb8fdbb
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61928705"
---
# <a name="instance-encoding-option"></a>執行個體編碼選項
**執行個體編碼選項**SQL 工作流程執行個體存放區的屬性可讓您指定 SQL 持續性提供者是否應該壓縮使用 GZip 演算法才能儲存工作流程執行個體狀態資訊將持續性資料庫的資訊。 此屬性允許的值為：GZip 和 None。 預設值為 None。 下列清單描述這些選項。  
  
1. **GZip**。 將狀態資訊保存在持續性資料庫中之前，持續性提供者會利用 GZip 演算法編碼狀態資訊。  
  
2. **None**： 將狀態資訊保存在持續性資料庫中之前，持續性提供者不會將狀態資訊編碼。  
  
 利用 GZip 編碼工作流程執行個體狀態資訊可以減少耗用 SQL 資料庫中的記憶體，同時也能減少耗用網路流量 (如果資料庫所在的電腦與執行工作流程服務主機的電腦位於不同的網路)。 一般的準則是將**執行個體編碼選項**屬性設**無**如果工作流程執行個體的狀態很小。
