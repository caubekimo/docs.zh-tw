---
title: 呈現控制項
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom controls [Windows Forms], rendering
- OnPaintBackground method [Windows Forms], invoking in Windows Forms custom controls
- custom controls [Windows Forms], graphics resources
- custom controls [Windows Forms], invalidation and painting
ms.assetid: aae8e1e6-4786-432b-a15e-f4c44760d302
ms.openlocfilehash: 0e1a7ca5dc0414d518c081b1db2dca5477d4f743
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742393"
---
# <a name="rendering-a-windows-forms-control"></a>呈現 Windows Form 控制項
呈現是指在使用者畫面上建立視覺表示的程式。 Windows Forms 使用 GDI （新的 Windows 圖形程式庫）來呈現。 提供 GDI 存取的 managed 類別位於 <xref:System.Drawing?displayProperty=nameWithType> 命名空間及其子命名空間中。  
  
 控制項呈現包含下列元素：  
  
- 基底類別所提供的繪製功能 <xref:System.Windows.Forms.Control?displayProperty=nameWithType>。  
  
- GDI 圖形程式庫的基本元素。  
  
- 繪製區域的幾何。  
  
- 釋放圖形資源的程式。  
  
## <a name="drawing-functionality-provided-by-control"></a>控制項所提供的繪圖功能  
 基底類別 <xref:System.Windows.Forms.Control> 透過其 <xref:System.Windows.Forms.Control.Paint> 事件提供繪製功能。 每當控制項需要更新其顯示時，就會引發 <xref:System.Windows.Forms.Control.Paint> 事件。 如需 .NET Framework 中事件的詳細資訊，請參閱[處理和引發事件](../../../standard/events/index.md)。  
  
 <xref:System.Windows.Forms.Control.Paint> 事件的事件資料類別，<xref:System.Windows.Forms.PaintEventArgs>會保存繪製控制項所需的資料，也就是繪圖物件的控制碼，以及代表要在其中繪製之區域的矩形物件。 這些物件會在下列程式碼片段中以粗體顯示。  
  
```vb  
Public Class PaintEventArgs  
   Inherits EventArgs  
   Implements IDisposable  
  
   Public ReadOnly Property ClipRectangle() As System.Drawing.Rectangle  
      ...  
   End Property  
  
   Public ReadOnly Property Graphics() As System.Drawing.Graphics  
      ...  
   End Property  
   ' Other properties and methods.  
   ...  
End Class  
```  
  
```csharp  
public class PaintEventArgs : EventArgs, IDisposable {  
public System.Drawing.Rectangle ClipRectangle {get;}  
public System.Drawing.Graphics Graphics {get;}  
// Other properties and methods.  
...  
}  
```  
  
 <xref:System.Drawing.Graphics> 是封裝繪製功能的 managed 類別，如本主題稍後的 GDI 討論中所述。 <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> 是 <xref:System.Drawing.Rectangle> 結構的實例，並定義控制項可以在其中繪製的可用區域。 控制項開發人員可以使用控制項的 [<xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A>] 屬性來計算 <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A>，如本主題稍後的 geometry 討論中所述。  
  
 控制項必須藉由覆寫繼承自 <xref:System.Windows.Forms.Control>的 <xref:System.Windows.Forms.Control.OnPaint%2A> 方法，來提供轉譯邏輯。 <xref:System.Windows.Forms.Control.OnPaint%2A> 取得繪圖物件和矩形的存取權，以透過傳遞給它的 <xref:System.Windows.Forms.PaintEventArgs> 實例的 <xref:System.Drawing.Design.PaintValueEventArgs.Graphics%2A> 和 <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> 屬性來繪製。  
  
```vb  
Protected Overridable Sub OnPaint(pe As PaintEventArgs)  
```  
  
```csharp  
protected virtual void OnPaint(PaintEventArgs pe);  
```  
  
 基底 <xref:System.Windows.Forms.Control> 類別的 <xref:System.Windows.Forms.Control.OnPaint%2A> 方法不會執行任何繪製功能，而只會叫用向 <xref:System.Windows.Forms.Control.Paint> 事件註冊的事件委派。 當您覆寫 <xref:System.Windows.Forms.Control.OnPaint%2A>時，通常應該叫用基類的 <xref:System.Windows.Forms.Control.OnPaint%2A> 方法，讓註冊的委派收到 <xref:System.Windows.Forms.Control.Paint> 事件。 不過，繪製其整個介面的控制項不應叫用基類的 <xref:System.Windows.Forms.Control.OnPaint%2A>，因為這會引進閃爍。 如需覆寫 <xref:System.Windows.Forms.Control.OnPaint%2A> 事件的範例，請參閱[如何：建立顯示進度的 Windows Forms 控制項](how-to-create-a-windows-forms-control-that-shows-progress.md)。  
  
> [!NOTE]
> 請勿直接從控制項叫用 <xref:System.Windows.Forms.Control.OnPaint%2A>;相反地，請叫用 <xref:System.Windows.Forms.Control.Invalidate%2A> 方法（繼承自 <xref:System.Windows.Forms.Control>）或其他會叫用 <xref:System.Windows.Forms.Control.Invalidate%2A>的方法。 <xref:System.Windows.Forms.Control.Invalidate%2A> 方法接著會叫用 <xref:System.Windows.Forms.Control.OnPaint%2A>。 <xref:System.Windows.Forms.Control.Invalidate%2A> 方法會多載，而且根據提供給 <xref:System.Windows.Forms.Control.Invalidate%2A> `e`的引數，控制項會重新繪製其部分或全部的螢幕區域。  
  
 基底 <xref:System.Windows.Forms.Control> 類別會定義另一個適用于繪製的方法，也就是 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 方法。  
  
```vb  
Protected Overridable Sub OnPaintBackground(pevent As PaintEventArgs)  
```  
  
```csharp  
protected virtual void OnPaintBackground(PaintEventArgs pevent);  
```  
  
 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 繪製視窗的背景（也就是圖形），而且保證會快速，而 <xref:System.Windows.Forms.Control.OnPaint%2A> 繪製詳細資料，而且可能會變慢，因為個別的繪製要求會合並成一個 <xref:System.Windows.Forms.Control.Paint> 事件，其中涵蓋所有必須重新繪製的區域。 例如，如果您想要繪製控制項的漸層色彩背景，您可能會想要叫用 <xref:System.Windows.Forms.Control.OnPaintBackground%2A>。  
  
 雖然 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 具有類似事件的命名法，並採用與 `OnPaint` 方法相同的引數，但 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 不是真正的事件方法。 沒有 `PaintBackground` 事件，而且 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 不會叫用事件委派。 覆寫 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 方法時，不需要衍生類別來叫用其基類的 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> 方法。  
  
## <a name="gdi-basics"></a>GDI + 基本概念  
 <xref:System.Drawing.Graphics> 類別提供繪製各種形狀（例如圓形、三角形、弧形和橢圓形）的方法，以及用來顯示文字的方法。 <xref:System.Drawing?displayProperty=nameWithType> 命名空間及其子命名空間包含的類別會封裝圖形元素，例如圖案（圓形、矩形、弧形等等）、色彩、字型、筆刷等等。 如需 GDI 的詳細資訊，請參閱[使用 Managed 圖形類別](../advanced/using-managed-graphics-classes.md)。 GDI 的基本知識也會在[如何：建立顯示進度的 Windows Forms 控制項](how-to-create-a-windows-forms-control-that-shows-progress.md)中描述。  
  
## <a name="geometry-of-the-drawing-region"></a>繪圖區域的幾何  
 控制項的 [<xref:System.Windows.Forms.Control.ClientRectangle%2A>] 屬性會指定使用者畫面上控制項可用的矩形區域，而 <xref:System.Windows.Forms.PaintEventArgs> 的 [<xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A>] 屬性則會指定實際繪製的區域。 （請記住，繪製作業是在採用 <xref:System.Windows.Forms.PaintEventArgs> 實例作為其引數的 <xref:System.Windows.Forms.Control.Paint> 事件方法中完成。 控制項可能只需要繪製其可用區域的一部分，如同控制項的小部分顯示變更的情況。 在這些情況下，控制項開發人員必須計算要在其中繪製的實際矩形，並將其傳遞給 <xref:System.Windows.Forms.Control.Invalidate%2A>。 採用 <xref:System.Drawing.Rectangle> 或 <xref:System.Drawing.Region> 做為引數之 <xref:System.Windows.Forms.Control.Invalidate%2A> 的多載版本，會使用該引數來產生 <xref:System.Windows.Forms.PaintEventArgs>的 <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> 屬性。  
  
 下列程式碼片段顯示 `FlashTrackBar` 的自訂控制項如何計算要在中繪製的矩形區域。 `client` 變數表示 <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> 屬性。 如需完整範例，請參閱[如何：建立顯示進度的 Windows Forms 控制項](how-to-create-a-windows-forms-control-that-shows-progress.md)。  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#6](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#6)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#6](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#6)]  
  
## <a name="freeing-graphics-resources"></a>釋放圖形資源  
 繪圖物件的成本很高，因為它們使用系統資源。 這類物件包括 <xref:System.Drawing.Graphics?displayProperty=nameWithType> 類別的實例，以及 <xref:System.Drawing.Brush?displayProperty=nameWithType>、<xref:System.Drawing.Pen?displayProperty=nameWithType>和其他圖形類別的實例。 只有在需要時才建立圖形資源，並在您完成使用它之後立即發行，這點很重要。 如果您建立的型別會執行 <xref:System.IDisposable> 介面，請在完成使用時呼叫它的 <xref:System.IDisposable.Dispose%2A> 方法，以便釋放資源。  
  
 下列程式碼片段顯示 `FlashTrackBar` 自訂控制項如何建立和釋放 <xref:System.Drawing.Brush> 資源。 如需完整的原始程式碼，請參閱[如何：建立顯示進度的 Windows Forms 控制項](how-to-create-a-windows-forms-control-that-shows-progress.md)。  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#5](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#5)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#5)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#4)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#4)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#3)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#3)]  
  
## <a name="see-also"></a>另請參閱

- [操作說明：建立顯示進度的 Windows Forms 控制項](how-to-create-a-windows-forms-control-that-shows-progress.md)
