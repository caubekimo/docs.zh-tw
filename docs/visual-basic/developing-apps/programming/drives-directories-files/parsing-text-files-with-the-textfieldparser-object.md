---
title: 使用 TextFieldParser 物件剖析文字檔
ms.date: 07/20/2015
helpviewer_keywords:
- TextFieldParser object, using
- I/O [Visual Basic], parsing files
- files [Visual Basic], parsing
ms.assetid: fc31d6e6-af0c-403f-8a00-d556b2c57567
ms.openlocfilehash: 7b2cb0dd39d14dd94db661cc9cbacf99edf0e778
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406673"
---
# <a name="parsing-text-files-with-the-textfieldparser-object-visual-basic"></a>使用 TextFieldParser 物件剖析文字檔 (Visual Basic)

`TextFieldParser` 物件可讓您剖析和處理非常大的檔案，以文字分隔欄寬為結構，例如記錄檔或舊版資料庫資訊。 使用 `TextFieldParser` 剖析文字檔案類似逐一查看文字檔案，而擷取文字欄位的剖析方法則類似用來語彙基元化分隔字串的字串操作方法。  
  
## <a name="parsing-different-types-of-text-files"></a>剖析不同型別的文字檔  

 文字檔欄位可有各種寬度，以逗點或定位點空格等字元分隔。 定義 `TextFieldType` 和分隔符號，如下例使用 `SetDelimiters` 方法來定義定位點分隔的文字檔︰  
  
 [!code-vb[VbVbalrTextFieldParser#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#21)]  
  
 其他文字檔可能有固定的欄位寬度。 在這種情況下，您需要將 `TextFieldType` 定義為 `FixedWidth`，並定義每個欄位的寬度，如下例所示。 本例使用 `SetFieldWidths` 方法來定義文字資料行︰第一個資料行是 5 個字元寬、第二個 10 個字元寬、第三個 11 個字元寬，第四個則為可變寬度。  
  
 [!code-vb[VbVbalrTextFieldParser#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#22)]  
  
 格式一旦定義，您就可以使用 `ReadFields` 方法循環往復處理檔案中的每一行。  
  
 如果欄位不符合指定的格式，將會擲回 <xref:Microsoft.VisualBasic.FileIO.MalformedLineException> 例外狀況。 擲回這類例外狀況時，`ErrorLine` 和 `ErrorLineNumber` 屬性會保留造成例外狀況及文字以及該文字的行號。  
  
## <a name="parsing-files-with-multiple-formats"></a>剖析有多種格式的檔案  

 `TextFieldParser` 物件的 `PeekChars` 方法可用來先檢查，然後再讀取每個欄位，讓您定義多種欄位格式，並據以採取動作。 如需詳細資訊，請參閱[如何：讀取多種格式的文字檔](how-to-read-from-text-files-with-multiple-formats.md)。  
  
## <a name="see-also"></a>另請參閱

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFieldParser%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.PeekChars%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ReadFields%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.CommentTokens%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.Delimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLineNumber%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.FieldWidths%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.HasFieldsEnclosedInQuotes%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.LineNumber%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TextFieldType%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TrimWhiteSpace%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetDelimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetFieldWidths%2A>
