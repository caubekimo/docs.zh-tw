---
title: "'System.Nullable(Of T)' 的方法不可以當做 'AddressOf' 運算子的運算元使用。"
ms.date: 07/20/2015
f1_keywords:
- vbc32126
- bc32126
helpviewer_keywords:
- BC32126
ms.assetid: 2325668b-e2ad-40ee-a1ec-30450236c20d
ms.openlocfilehash: 61c6fe7c33b3292066e653304ded43a863413723
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397216"
---
# <a name="methods-of-systemnullableof-t-cannot-be-used-as-operands-of-the-addressof-operator"></a>'System.Nullable(Of T)' 的方法不可以當做 'AddressOf' 運算子的運算元使用。
語句使用運算子搭配 `AddressOf` 代表結構程式的運算元 <xref:System.Nullable%601> 。  
  
 **錯誤識別碼：** BC32126  
  
## <a name="to-correct-this-error"></a>更正這個錯誤  
  
- 將子句中的程式名稱取代 `AddressOf` 為不是成員的運算元 <xref:System.Nullable%601> 。  
  
- 撰寫類別，以包裝 <xref:System.Nullable%601> 您要使用的方法。 在下列範例中， `NullableWrapper` 類別會定義名為的新方法 `GetValueOrDefault` 。 因為這個新方法不是的成員 <xref:System.Nullable%601> ，所以可以套用至可 `nullInstance` 為 null 之型別的實例，以形成的引數 `AddressOf` 。  
  
```vb  
Module Module1  
  
    Delegate Function Deleg() As Integer  
  
    Sub Main()  
        Dim nullInstance As New Nullable(Of Integer)(1)  
  
        Dim del As Deleg  
  
        ' GetValueOrDefault is a method of the Nullable generic  
        ' type. It cannot be used as an operand of AddressOf.  
        ' del = AddressOf nullInstance.GetValueOrDefault  
  
        ' The following line uses the GetValueOrDefault method  
        ' defined in the NullableWrapper class.  
        del = AddressOf (New NullableWrapper(  
            Of Integer)(nullInstance)).GetValueOrDefault  
  
        Console.WriteLine(del.Invoke())  
    End Sub  
  
    Class NullableWrapper(Of T As Structure)  
        Private m_Value As Nullable(Of T)  
  
        Sub New(ByVal Value As Nullable(Of T))  
            m_Value = Value  
        End Sub  
  
        Public Function GetValueOrDefault() As T  
            Return m_Value.Value  
        End Function  
    End Class  
End Module  
```  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Nullable%601>
- [AddressOf 運算子](../operators/addressof-operator.md)
- [可為 null 的實數值型別](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
