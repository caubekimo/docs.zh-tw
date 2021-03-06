---
title: 如何：使用 ToolStrip 控制項中的工具提示
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- AutoComplete [Windows Forms], examples
- toolbars [Windows Forms], AutoComplete
- examples [Windows Forms], toolbars
- AutoComplete [Windows Forms], enabling in toolbars
- ToolStripComboBox class [Windows Forms], examples
- ToolStrip control [Windows Forms], AutoComplete
ms.assetid: fd66d085-1af1-45d4-930a-cde944da2e16
ms.openlocfilehash: 18b17aaea9d2354c03bb43f3fdd8d3779697cf58
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79142014"
---
# <a name="how-to-enable-autocomplete-in-toolstrip-controls-in-windows-forms"></a>如何：啟用 Windows Form 中 ToolStrip 控制項的 AutoComplete
以下過程將 和<xref:System.Windows.Forms.ToolStripLabel><xref:System.Windows.Forms.ToolStripComboBox>可以向下刪除的 將 合併為 顯示專案清單（如最近訪問的網站）。 如果使用者鍵入與清單中某一項的第一個字元匹配的字元，則該項將立即顯示。  
  
> [!NOTE]
> 自動完成的工作方式`ToolStrip`與使用傳統控制項（如 和<xref:System.Windows.Forms.ComboBox><xref:System.Windows.Forms.TextBox>） 相同。  
  
### <a name="to-enable-autocomplete-in-a-toolstrip-control"></a>在工具條控制項中啟用自動完成  
  
1. 創建控制項<xref:System.Windows.Forms.ToolStrip>並將其添加項。  
  
    ```vb  
    ToolStrip1 = New System.Windows.Forms.ToolStrip  
    ToolStrip1.Items.AddRange(New System.Windows.Forms.ToolStripItem()_  
        {ToolStripLabel1, ToolStripComboBox1})  
    ```  
  
    ```csharp  
    toolStrip1 = new System.Windows.Forms.ToolStrip();  
    toolStrip1.Items.AddRange(new System.Windows.Forms.ToolStripItem[]
        {toolStripLabel1, toolStripComboBox1});  
    ```  
  
2. 將<xref:System.Windows.Forms.ToolStripItem.Overflow%2A>標籤和下拉式列示方塊的屬性設置為<xref:System.Windows.Forms.ToolStripItemOverflow.Never>，以便無論表單的大小如何，清單始終可用。  
  
    ```vb  
    ToolStripLabel1.Overflow = _  
        System.Windows.Forms.ToolStripItemOverflow.Never  
    ToolStripComboBox1.Overflow = _  
        System.Windows.Forms.ToolStripItemOverflow.Never  
    ```  
  
    ```csharp  
    toolStripLabel1.Overflow = _  
        System.Windows.Forms.ToolStripItemOverflow.Never  
    toolStripComboBox1.Overflow = System.Windows.Forms.ToolStripItemOverflow.Never  
    ```  
  
3. 向<xref:System.Windows.Forms.ToolStripComboBox>控制項的"項"集合添加單詞。  
  
    ```vb  
    ToolStripComboBox1.Items.AddRange(New Object() {"First Item", _  
        "Second Item", "Third Item"})  
    ```  
  
    ```csharp  
    toolStripComboBox1.Items.AddRange(new object[] {"First item", "Second item", "Third item"});  
    ```  
  
4. 將<xref:System.Windows.Forms.ComboBox.AutoCompleteMode%2A>下拉式列示方塊的屬性設置為<xref:System.Windows.Forms.AutoCompleteMode.Append>。  
  
    ```vb  
    ToolStripComboBox1.AutoCompleteMode = _  
        System.Windows.Forms.AutoCompleteMode.Append  
    ```  
  
    ```csharp  
    toolStripComboBox1.AutoCompleteMode = System.Windows.Forms.AutoCompleteMode.Append;  
    ```  
  
5. 將<xref:System.Windows.Forms.ComboBox.AutoCompleteSource%2A>下拉式列示方塊的屬性設置為<xref:System.Windows.Forms.AutoCompleteSource.ListItems>。  
  
    ```vb  
    ToolStripComboBox1.AutoCompleteSource = _  
        System.Windows.Forms.AutoCompleteSource.ListItems  
    ```  
  
    ```csharp  
    toolStripComboBox1.AutoCompleteSource = System.Windows.Forms.AutoCompleteSource.ListItems;  
    ```  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.ToolStripLabel>
- <xref:System.Windows.Forms.ToolStripComboBox>
- <xref:System.Windows.Forms.ToolStripComboBox.AutoCompleteMode%2A>
- <xref:System.Windows.Forms.ToolStripComboBox.AutoCompleteSource%2A>
- [ToolStrip 控制項概觀](toolstrip-control-overview-windows-forms.md)
- [ToolStrip 控制項架構](toolstrip-control-architecture.md)
- [ToolStrip 技術摘要](toolstrip-technology-summary.md)
