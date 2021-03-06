---
title: 如何使用屬性 （C#） 創建 C/C++聯合
ms.date: 07/20/2015
ms.assetid: 85f35e56-26e0-4d31-9f3a-89bd4005e71a
ms.openlocfilehash: ff8ce560444581a28b257820573224f89a274cd9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141582"
---
# <a name="how-to-create-a-cc-union-by-using-attributes-c"></a>如何使用屬性 （C#） 創建 C/C++聯合

通過使用屬性，可以自訂結構在記憶體中的佈局方式。 例如，您可以使用 `StructLayout(LayoutKind.Explicit)` 和 `FieldOffset` 屬性，以 C/C++ 建立所謂的等位。

## <a name="example"></a>範例

在此程式碼片段中，`TestUnion` 的所有欄位都在記憶體的同一位置開始。

```csharp
// Add a using directive for System.Runtime.InteropServices.

[System.Runtime.InteropServices.StructLayout(LayoutKind.Explicit)]
struct TestUnion
{
    [System.Runtime.InteropServices.FieldOffset(0)]
    public int i;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public double d;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public char c;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public byte b;
}
```

## <a name="example"></a>範例

以下是另一個範例，欄位從明確設定的不同位置開始。

```csharp
// Add a using directive for System.Runtime.InteropServices.

[System.Runtime.InteropServices.StructLayout(LayoutKind.Explicit)]
struct TestExplicit
{
    [System.Runtime.InteropServices.FieldOffset(0)]
    public long lg;

    [System.Runtime.InteropServices.FieldOffset(0)]
    public int i1;

    [System.Runtime.InteropServices.FieldOffset(4)]
    public int i2;

    [System.Runtime.InteropServices.FieldOffset(8)]
    public double d;

    [System.Runtime.InteropServices.FieldOffset(12)]
    public char c;

    [System.Runtime.InteropServices.FieldOffset(14)]
    public byte b;
}
```

`i1` 和 `i2` 這兩個整數欄位和 `lg` 共用相同的記憶體位置。 使用平台叫用時，這種結構配置控制項很有用。

## <a name="see-also"></a>另請參閱

- <xref:System.Reflection>
- <xref:System.Attribute>
- [C# 程式設計指南](../../index.md)
- [屬性](../../../../standard/attributes/index.md)
- [反映 (C#)](../reflection.md)
- [屬性 (C#)](index.md)
- [建立自訂屬性 (C#)](creating-custom-attributes.md)
- [使用反映存取屬性 (C#)](accessing-attributes-by-using-reflection.md)
