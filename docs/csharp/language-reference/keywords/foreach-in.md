---
title: C# foreach ステートメント
ms.date: 06/29/2018
f1_keywords:
- foreach
- foreach_CSharpKeyword
helpviewer_keywords:
- foreach keyword [C#]
- foreach statement [C#]
- in keyword [C#]
ms.assetid: 5a9c5ddc-5fd3-457a-9bb6-9abffcd874ec
ms.openlocfilehash: 417a8cefbc9bc7544ae1156992e6e6c549fb828f
ms.sourcegitcommit: ccd8c36b0d74d99291d41aceb14cf98d74dc9d2b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2018
ms.locfileid: "53128623"
---
# <a name="foreach-in-c-reference"></a>foreach、in (C# リファレンス)

`foreach` ステートメントは、<xref:System.Collections.IEnumerable?displayProperty=nameWithType> または <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> インターフェイスを実装している型のインスタンス内の要素ごとに、ステートメントまたはステートメントのブロックを実行します。 `foreach` ステートメントはこのような型にのみ限定されるものではありません。次の条件を満たす任意の型のインスタンスに適用できます。

- 戻り値の型がクラス、構造体、インターフェイス型のいずれかである、パラメーターなしのパブリック メソッド `GetEnumerator` を持っている。
- `GetEnumerator` メソッドの戻り値の型が、パブリック プロパティ `Current` と、戻り値の型が <xref:System.Boolean> であるパラメーターなしのパブリック メソッド `MoveNext` を持っている。

C# 7.3 以降、列挙子の `Current` プロパティが、[参照戻り値](ref.md#reference-return-values) (`ref T``T` はコレクション要素の種類です) を返す場合、`ref` または `ref readonly` 修飾子を使用して繰り返し変数を宣言することができます。

`foreach` ステートメント ブロック内の任意の位置で、[break](break.md) ステートメントを使ってループから抜けることができます。または、[continue](continue.md) ステートメントを使って、ループ内の次の繰り返しにスキップできます。 また、[goto](goto.md)、[return](return.md)、[throw](throw.md) ステートメントのいずれかを使って `foreach` ループを終了することもできます。

## <a name="examples"></a>使用例

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

次の例では、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装する <xref:System.Collections.Generic.List%601> 型のインスタンスを使用して、`foreach` ステートメントの使用方法を示します。

[!code-csharp-interactive[list example](~/samples/snippets/csharp/keywords/IterationKeywordsExamples.cs#1)]

次の例では、何のインターフェイスも実装していない <xref:System.Span%601?displayProperty=nameWithType> 型のインスタンスを使用して、`foreach` ステートメントを使用します。

[!code-csharp-interactive[span example](~/samples/snippets/csharp/keywords/IterationKeywordsExamples.cs#2)]

次の例では、stackalloc 配列内の各項目の値を設定するために、`ref` 繰り返し変数を使用します。 `ref readonly` バージョンは、すべての値を出力するコレクションを反復処理します。 `readonly` 宣言では、暗黙のローカル変数宣言を使用します。 暗黙の変数宣言は、`ref` または `ref readonly` 宣言で使用でき、変数の宣言を明示的に型指定できます。

[!code-csharp-interactive[ref span example](~/samples/snippets/csharp/keywords/IterationKeywordsExamples.cs#RefSpan)]

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](../language-specification/index.md)」の [foreach ステートメント](~/_csharplang/spec/statements.md#the-foreach-statement)に関するセクションを参照してください。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# プログラミング ガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [繰り返しステートメント](iteration-statements.md)
- [配列での foreach の使用](../../programming-guide/arrays/using-foreach-with-arrays.md)
- [for ステートメント](for.md)
