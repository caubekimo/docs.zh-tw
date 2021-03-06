---
title: 操作說明：使用 From、To 和 By 控制動畫
ms.date: 03/30/2017
helpviewer_keywords:
- animation [WPF], From/to/by
- basic animation [WPF]
- animation [WPF], basic animation
- From/to/by animation
ms.assetid: 59afba57-6fc1-44c8-987e-8a5f4142adad
ms.openlocfilehash: b06df97dc57c58a01f30be2d70bfeebf104521ad
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77451781"
---
# <a name="how-to-control-an-animation-using-from-to-and-by"></a>操作說明：使用 From、To 和 By 控制動畫
「From/To/By」或「基本動畫」會建立兩個目標值之間的轉換（如需不同動畫類型的簡介，請參閱[動畫總覽](animation-overview.md)）。 若要設定基本動畫的目標值，請使用其 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>和 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 屬性。  下表摘要說明如何同時或個別使用 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>和 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 屬性來判斷動畫的目標值。  
  
|指定的屬性|產生的行為|  
|--------------------------|------------------------|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>|動畫會從 [<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>] 屬性所指定的值，到所要繪製之屬性的基底值，或上一個動畫的輸出值進行，視上一個動畫的設定方式而定。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> 和 <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>|動畫會從 [<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>] 屬性所指定的值進行到 [<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>] 屬性所指定的值。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> 和 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>|動畫會從 [<xref:System.Windows.Media.Animation.DoubleAnimation.From%2A>] 屬性所指定的值，到 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> 和 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 屬性總和所指定的值進行。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>|動畫會從動畫屬性的基底值或前一個動畫的輸出值進行，到 <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> 屬性所指定的值。|  
|<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>|動畫會從正在製作動畫之屬性的基底值，或上一個動畫的輸出值，到該值的總和和 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 屬性所指定的值。|  
  
> [!NOTE]
> 請勿在相同的動畫上同時設定 <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A> 屬性和 <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> 屬性。  
  
 若要使用其他插補方法，或建立兩個以上的目標值之間的動畫，請使用主要畫面格動畫。 如需詳細資訊，請參閱[主要畫面格動畫總覽](key-frame-animations-overview.md)。  
  
 如需將多個動畫套用至單一屬性的詳細資訊，請參閱[主要畫面格動畫總覽](key-frame-animations-overview.md)。  
  
 下列範例顯示在動畫上設定 <xref:System.Windows.Media.Animation.DoubleAnimation.To%2A>、<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>和 <xref:System.Windows.Media.Animation.DoubleAnimation.From%2A> 屬性的不同效果。  
  
## <a name="example"></a>範例  
 [!code-xaml[BasicAnimations_snippet#AnimationTargetValuesWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/BasicAnimations_snippet/CS/AnimationTargetValuesExample.xaml#animationtargetvalueswholepage)]  
  
## <a name="see-also"></a>另請參閱

- [動畫概觀](animation-overview.md)
- [主要畫面格動畫概觀](key-frame-animations-overview.md)
- [From、To 和 By 動畫目標值範例](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/TargetValues)
