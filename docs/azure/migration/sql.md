---
title: 將 SQL Server 資料庫移轉至 Azure
description: 了解如何從內部部署 SQL Server 將 SQL Server 資料庫移轉到 Azure。
ms.topic: how-to
ms.date: 05/27/2020
ms.openlocfilehash: ed5d6ef9395dca14d8e0ecba82d3fc18cb3d629a
ms.sourcegitcommit: a241301495a84cc8c64fe972330d16edd619868b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2020
ms.locfileid: "84241444"
---
# <a name="migrate-a-sql-server-database-to-azure"></a>將 SQL Server 資料庫移轉至 Azure

本文提供兩個選項的簡要概述，可將 SQL Server 資料庫移轉至 Azure。 Azure 有三個主要選項可用於遷移生產 SQL Server 資料庫。 本文著重于下列兩個選項：

1. [Azure VM 上的 SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)：安裝並裝載在 Azure 中執行的 Windows 虛擬機器上的 SQL Server 執行個體，也稱為基礎結構即服務 (IaaS)。
2. [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)：完全受控的 SQL 資料庫 Azure 服務，也稱為平台即服務 (PaaS)。

兩者皆具優缺點，您必須在移轉前進行評估。 第三個選項是[Azure SQL Database 受控實例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)。

## <a name="get-started"></a>開始使用

根據您使用的服務選擇下列實用的移轉指南：

* [將 SQL Server 資料庫移轉至 Azure VM 中的 SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)
* [將 SQL Server Database 移轉至 Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-migrate-your-sql-server-database)

此外，下列連結提供概念性的內容，將協助您進一步了解 VM：

* [Azure 虛擬機器中的 SQL Server 高可用性和災害復原](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)
* [Azure 虛擬機器中的 SQL Server 效能最佳做法](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)
* [Azure 虛擬機器中的 SQL Server 應用程式模式和開發策略](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-app-patterns-dev-strategies)

下列連結可協助您進一步了解 Azure SQL Database：

* [建立和管理 Azure SQL Database 伺服器與資料庫](https://docs.microsoft.com/azure/sql-database/sql-database-servers-databases)
* [資料庫交易單位 (DTU) 和彈性資料庫交易單位 (eDTU)](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)
* [Azure SQL Database 資源限制](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits)

## <a name="choosing-iaas-or-paas"></a>選擇 IaaS 或 PaaS

在評估要遷移資料庫的位置時，請判斷 IaaS 或 PaaS 是否較適合您。

**如果有下列情況，請選擇 Azure Vm 中的 SQL Server：**

* 您想要「隨即轉移」資料庫和應用程式，且讓變更最少或無變更。
* 您想要完整控制資料庫伺服器和其執行位置所在的 VM。
* 您已擁有打算使用的 SQL Server 和 Windows Server 授權。

**如果是下列情形，請選擇 Azure SQL Database ：**

* 您想要將應用程式現代化，且正在移轉以使用其他 Azure 中的 PaaS 服務。
* 您不想管理資料庫伺服器和其執行位置所在的 VM。
* 您沒有 SQL Server 或 Windows Server 授權，或您打算讓擁有的授權到期。

下表根據各組案例說明每個服務之間的差異。

| 狀況 | Azure VM 中的 SQL Server | Azure SQL Database |
|----------|-------------------------|--------------------|
| 遷移 | 資料庫所需變更最少。 | 如果您使用 Azure SQL 中無法使用的功能 (由[資料移轉小幫手](https://www.microsoft.com/download/details.aspx?id=53595)判定)，或如果您有其他相依性，如在本機安裝的可執行檔，則可能需要變更資料庫。|
| 管理可用性、復原及升級 | 可用性和復原是以手動方式設定。 升級可以透過 [VM 擴展集](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade)自動化。 | 為您自動管理。 |
| 基礎作業系統設定 | 手動設定。 | 為您自動管理。 |
| 管理資料庫大小 | 針對每個 SQL Server 實例最多支援 256 TB 的儲存體。 | 在需要水準資料分割之前，支援 8 TB 的儲存空間。 |
| 管理成本 | 您必須管理 SQL Server 授權成本、Windows Server 授權成本以及 VM 成本 (根據核心、RAM 和儲存體)。 | 您必須管理服務成本 (根據 [eDTU 或 DTU](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu)、儲存體以及資料庫數目 (如果使用彈性集區))。 您也必須管理任何 SLA 的成本。 |

若要深入瞭解兩者之間的差異，請參閱[在 AZURE SQL 中選擇正確的部署選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas)。

## <a name="faq"></a>常見問題集

* **我仍然可以使用如 SQL Server Management Studio 和 SQL Server Reporting Services (SSRS) 等工具，搭配 Azure VM 中的 SQL Server 或 Azure SQL Database 嗎？**

    是。 所有 Microsoft SQL 工具皆可搭配使用這兩項服務。 但 SSRS 並非 Azure SQL Database 的一部分，建議您在 Azure VM 中執行它，然後將其指向您的資料庫執行個體。

* **我想要用 PaaS，但我不確定資料庫是否相容。有工具可以協助嗎？**

    是。 [資料移轉小幫手](https://www.microsoft.com/download/details.aspx?id=53595)是移轉到 Azure SQL Database 的一部分所用工具。 [Azure 資料庫移轉服務](https://azure.microsoft.com/campaigns/database-migration/)是一項預覽服務，可供您用於 IaaS 或 PaaS。

* **我可以預估成本嗎？**

    是。 [Azure 價格計算機](https://azure.microsoft.com/pricing/calculator/)可針對所有 Azure 服務 (包括 VM 及資料庫服務) 進行成本預估。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [選擇正確的 Azure 裝載選項](choose.md)
