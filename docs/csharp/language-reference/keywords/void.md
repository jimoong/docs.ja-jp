---
title: void - C# リファレンス
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- void_CSharpKeyword
- void
helpviewer_keywords:
- void keyword [C#]
ms.assetid: 0d2d8a95-fe20-4fbd-bf5d-c1e54bce71d4
ms.openlocfilehash: ce52e4d19335db0e21637b87bbd1589c86c06ae1
ms.sourcegitcommit: fa38fe76abdc8972e37138fcb4dfdb3502ac5394
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2018
ms.locfileid: "53613129"
---
# <a name="void-c-reference"></a>void (C# リファレンス)

メソッドの戻り値の型として使用した場合、`void` はメソッドが値を返さないことを指定します。

`void` は、メソッドのパラメーター リストに含めることはできません。 パラメーターを取らず、値を返さないメソッドは、次のように宣言されます。

```csharp
public void SampleMethod()
{
    // Body of the method.
}
```

`void` は、不明な型へのポインターを宣言するために、unsafe コンテキストでも使用されます。 詳しくは、「[ポインター型](../../programming-guide/unsafe-code-pointers/pointer-types.md)」をご覧ください。

`void` は、.NET Framework の <xref:System.Void?displayProperty=nameWithType> 型のエイリアスです。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# プログラミング ガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [参照型](reference-types.md)
- [値型](value-types.md)
- [メソッド](../../programming-guide/classes-and-structs/methods.md)
- [アンセーフ コードとポインター](../../programming-guide/unsafe-code-pointers/index.md)