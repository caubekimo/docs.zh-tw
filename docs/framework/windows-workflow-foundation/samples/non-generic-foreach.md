---
title: 非泛型 ForEach
ms.date: 03/30/2017
ms.assetid: 576cd07a-d58d-4536-b514-77bad60bff38
ms.openlocfilehash: 08dbac3974915f823a4f6e39f35927453a7c4b3a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79142702"
---
# <a name="non-generic-foreach"></a>非泛型 ForEach
[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 的工具箱中隨附一組控制流程活動，包括允許逐一查看 <xref:System.Activities.Statements.ForEach%601> 集合的 <xref:System.Collections.Generic.IEnumerable%601>。  
  
 <xref:System.Activities.Statements.ForEach%601> 的 <xref:System.Activities.Statements.ForEach%601.Values%2A> 屬性必須是 <xref:System.Collections.Generic.IEnumerable%601> 型別。 這會防止使用者逐一查看實作 <xref:System.Collections.Generic.IEnumerable%601> 介面的資料結構 (例如 <xref:System.Collections.ArrayList>)。 非泛型 <xref:System.Activities.Statements.ForEach%601> 版本沒有這項需求，但在執行階段更複雜，以確保集合中數值型別的相容性。  
  
 這個範例示範如何實作非泛型 <xref:System.Activities.Statements.ForEach%601> 活動及其設計工具。 這個活動可用來逐一查看 <xref:System.Collections.ArrayList>。  
  
## <a name="foreach-activity"></a>ForEach 活動  
 C#/Visual Basic`foreach`語句枚舉集合的元素，為集合的每個元素執行嵌入語句。 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 的 `foreach` 對應活動為 <xref:System.Activities.Statements.ForEach%601> 和 <xref:System.Activities.Statements.ParallelForEach%601>。 <xref:System.Activities.Statements.ForEach%601> 活動包含值清單和主體。 在執行階段，會列舉此清單，並針對清單中的每個值執行主體。  
  
 在大部分情況下，泛型活動版本應該是慣用方案，因為它涵蓋大部分使用狀況，也提供編譯階段的類型檢查。 非泛型版本可用來逐一查看實作非泛型 <xref:System.Collections.IEnumerable> 介面的型別。  
  
## <a name="class-definition"></a>類別定義  
 下列程式碼範例示範非泛型 `ForEach` 活動的定義。  
  
```csharp  
[ContentProperty("Body")]  
public class ForEach : NativeActivity  
{  
    [RequiredArgument]  
    [DefaultValue(null)]  
    InArgument<IEnumerable> Values { get; set; }  
  
    [DefaultValue(null)]  
    [DependsOn("Values")]  
    ActivityAction<object> Body { get; set; }
}  
```  
  
 Body (選擇性)  
 <xref:System.Activities.ActivityAction> 型別的 <xref:System.Object>，針對集合中的每個項目來執行。 每個個別項目透過 `Argument` 屬性傳遞至 Body。  
  
 Values (選擇性)  
 逐一查看的項目集合。 確認所有集合項目具有相容型別的作業是在執行階段進行。  
  
## <a name="example-of-using-foreach"></a>使用 ForEach 的範例  
 下列程式碼示範如何在應用程式中使用 ForEach 活動。  
  
```csharp  
string[] names = { "bill", "steve", "ray" };  
  
DelegateInArgument<object> iterationVariable = new DelegateInArgument<object>() { Name = "iterationVariable" };  
  
Activity sampleUsage =  
    new ForEach  
    {  
       Values = new InArgument<IEnumerable>(c=> names),  
       Body = new ActivityAction<object>
       {
           Argument = iterationVariable,  
           Handler = new WriteLine  
           {  
               Text = new InArgument<string>(env => string.Format("Hello {0}",                                                               iterationVariable.Get(env)))  
           }  
       }  
   };  
```  
  
|條件|訊息|Severity|例外狀況類型|  
|---------------|-------------|--------------|--------------------|  
|Values 為 `null`。|未提供必要活動引數 'Values' 的值。|錯誤|<xref:System.InvalidOperationException>|  
  
## <a name="foreach-designer"></a>ForEach 設計工具  
 此範例的活動設計工具在外觀上與針對內建 <xref:System.Activities.Statements.ForEach%601> 活動所提供的設計工具類似。 設計器將顯示在 **"示例**、**非通用活動"** 類別中的工具框中。 設計器在工具箱中名為**ForEachWithBodyFactory，** 因為活動會在工具箱中<xref:System.Activities.Presentation.IActivityTemplateFactory>公開 一個，該工具箱使用正確配置<xref:System.Activities.ActivityAction>創建活動。  
  
```csharp  
public sealed class ForEachWithBodyFactory : IActivityTemplateFactory  
{  
    public Activity Create(DependencyObject target)  
    {  
        return new Microsoft.Samples.Activities.Statements.ForEach()  
        {  
            Body = new ActivityAction<object>()  
            {  
                Argument = new DelegateInArgument<object>()  
                {  
                    Name = "item"  
                }  
            }  
        };  
    }  
}  
```  
  
#### <a name="to-run-this-sample"></a>執行此範例  
  
1. 將您選擇的專案設定為方案的啟始專案：  
  
    1. **代碼測試用戶端**演示如何使用代碼使用活動。  
  
    2. **設計器測試用戶端**演示如何在設計器中使用活動。  
  
2. 建置並執行專案。  
  
> [!IMPORTANT]
> 這些範例可能已安裝在您的電腦上。 請先檢查下列 (預設) 目錄，然後再繼續。  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目錄不存在，請轉到[Windows 通信基礎 （WCF） 和 Windows 工作流基礎 （WF） 示例 .NET 框架 4](https://www.microsoft.com/download/details.aspx?id=21459)以下載[!INCLUDE[wf1](../../../../includes/wf1-md.md)]所有 Windows 通信基礎 （WCF） 和示例。 此範例位於下列目錄。  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\NonGenericForEach`
