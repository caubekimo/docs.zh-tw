---
title: 在 Windows 10、8.1、8 上安裝 .NET 框架 3.5
description: 了解如何在 Windows 10、Windows 8.1 及 Windows 8 上安裝 .NET Framework 3.5。
ms.date: 07/16/2018
ms.openlocfilehash: cfe21c0821b8f3223301dcc802533e1aaf024a79
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2020
ms.locfileid: "76965941"
---
# <a name="install-the-net-framework-35-on-windows-10-windows-81-and-windows-8"></a>在 Windows 10、Windows 8.1 及 Windows 8 上安裝 .NET Framework 3.5

您可能需要 .NET Framework 3.5，才能在 Windows 10、Windows 8.1 及 Windows 8 上執行應用程式。 這些指示也適用於較舊的 Windows 版本。

## <a name="download-the-offline-installer"></a>下載離線安裝程式

.NET 框架 3.5 SP1 離線安裝程式在[.NET 框架 3.5 SP1 下載頁面上](https://dotnet.microsoft.com/download/dotnet-framework/net35-sp1)可用，並且可用於 Windows 10 之前的 Windows 版本。

## <a name="install-the-net-framework-35-on-demand"></a>視需要安裝 .NET Framework 3.5

如果您嘗試執行需要 .NET Framework 3.5 的應用程式，可能會看到下列設定對話方塊。 請選擇 [安裝此功能]**** 來啟用 .NET Framework 3.5。 這個選項需要網際網路連線。

![.NET 框架安裝對話方塊的螢幕截圖。](./media/dotnet-35-windows-10/dotnet-framework-installation-dialog.png)

### <a name="why-am-i-getting-this-pop-up"></a>為什麼我會收到這個快顯？

.NET Framework 由 Microsoft 建立，並提供執行應用程式的環境。 有不同的版本可以使用。 許多公司開發使用 .NET Framework 執行的應用程式，而這些應用程式以特定版本為目標。 如果您看到這個這個快顯視窗，表示您正在執行一個需要 .NET Framework 3.5 版的應用程式，但您的系統上並未安裝該版本。

## <a name="enable-the-net-framework-35-in-control-panel"></a>在控制台中啟用 .NET Framework 3.5

您可以透過 Windows 的 [控制台] 啟用 .NET Framework 3.5。 這個選項需要網際網路連線。

1. 按 Windows![金鑰徽標的 Windows 金鑰螢幕截圖。](./media/dotnet-35-windows-10/windows-keyboard-logo.png) 在鍵盤上，鍵入"Windows 功能"，然後按 Enter。 [開啟或關閉 Windows 功能]**** 對話方塊隨即出現。

2. 選取 [.NET Framework 3.5 (包括 .NET 2.0 和 3.0)]**** 核取方塊，選取 [確定]****，然後在出現提示時重新啟動電腦。

   ![顯示使用控制台安裝 .NET 的螢幕截圖。](./media/dotnet-35-windows-10/dotnet-control-panel.png)

   您不需要選取 [Windows Communication Foundation (WCF) HTTP 啟用]**** 和 [Windows Communication Foundation (WCF) 非 HTTP 啟用]**** 的子項目，除非您是需要這項功能的開發人員或伺服器系統管理員。

## <a name="troubleshoot-the-installation-of-the-net-framework-35"></a>進行 .NET Framework 3.5 安裝的疑難排解

在安裝過程中，可能會出現錯誤 0x800f0906、0x800f0907、0x800f081f 或 0x800F0922，如果發生這種情況，請參考 [.NET Framework 3.5 安裝錯誤：0x800f0906、0x800f0907 或 0x800f081f](https://support.microsoft.com/help/2734782/net-framework-3-5-installation-error-0x800f0906--0x800f081f--0x800f09)，了解如何解決這些問題。

如果您仍無法解決您的安裝問題，或者您沒有網際網路連線，則可以嘗試使用 Windows 安裝媒體進行安裝。 如需詳細資訊，請參閱[使用部署映像服務與管理 (DISM) 部署 .NET Framework 3.5](/windows-hardware/manufacture/desktop/deploy-net-framework-35-by-using-deployment-image-servicing-and-management--dism)。 如果您沒有安裝媒體，請參閱[建立 Windows 的安裝媒體](https://support.microsoft.com/help/15088/windows-create-installation-media)。

> [!WARNING]
> 如果您不依賴 Windows Update 做為安裝.NET Framework 3.5 的來源，則必須確定務必使用對應到相同 Windows 作業系統版本的來源。 使用未對應到相同 Windows 版本的來源路徑不會防止安裝不相符的 NET Framework 3.5 版本。 不過，這會導致系統不受支援且無法提供服務的狀態。
