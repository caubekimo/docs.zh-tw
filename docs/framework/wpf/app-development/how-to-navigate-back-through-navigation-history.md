---
title: HOW TO：在瀏覽記錄中向後巡覽
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- history [WPF], navigating back
- navigation [WPF], through navigation history (back)
ms.assetid: 9343234b-d864-441d-b8a7-d895cba80a87
ms.openlocfilehash: 53b32e145390d7052262042c7a793699c163b373
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69969358"
---
# <a name="how-to-navigate-back-through-navigation-history"></a>作法：在瀏覽記錄中向後巡覽
此範例說明如何流覽至後端導覽歷程記錄中的專案。  
  
## <a name="example"></a>範例  
 從裝載于<xref:System.Windows.Navigation.NavigationWindow>、 <xref:System.Windows.Controls.Frame>使用<xref:System.Windows.Navigation.NavigationService>或 Internet Explorer 的內容執行的程式碼, 可以透過導覽歷程記錄 (一次一個專案) 導覽回來。  
  
 您必須先檢查**CanGoBack**屬性, 然後藉由呼叫**GoBack**方法, 在流覽回一個專案之前, 先確認上一頁導覽歷程記錄中有專案, 然後再流覽一個專案。 如下列範例所示:  
  
 [!code-csharp[HOWTONavigationSnippets#NavigateBackCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/HOWTONavigationSnippets/CSharp/HomePage.xaml.cs#navigatebackcode)]
 [!code-vb[HOWTONavigationSnippets#NavigateBackCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HOWTONavigationSnippets/visualbasic/homepage.xaml.vb#navigatebackcode)]  
  
 **CanGoBack**和**GoBack**是由<xref:System.Windows.Navigation.NavigationWindow>、 <xref:System.Windows.Controls.Frame>和<xref:System.Windows.Navigation.NavigationService>所執行。  
  
> [!NOTE]
> 如果您呼叫**GoBack**, 而且後端導覽歷程記錄中沒有任何專案, <xref:System.InvalidOperationException>則會引發。
