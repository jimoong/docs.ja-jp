---
title: LINQ と文字列 (C#)
ms.date: 07/20/2015
ms.assetid: dbe2d657-b3f3-487e-b645-21fb2d71cd7b
ms.openlocfilehash: c7a1b86cc611d5f38ceab814b4594f5ad953fbc4
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54744623"
---
# <a name="linq-and-strings-c"></a>LINQ と文字列 (C#)

文字列やそのコレクションは、LINQ を使って照会したり変換したりすることができます。 特に、テキスト ファイル内の半構造化されたデータでその利便性が発揮されます。 LINQ クエリは、従来の文字列関数や正規表現と組み合わせることができます。 たとえば、<xref:System.String.Split%2A?displayProperty=nameWithType> または <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> メソッドを使用して、文字列の配列を作成し、その後で LINQ を使用してクエリを実行したり変更したりすることができます。 LINQ クエリの `where` 句で <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> メソッドを使用できます。 LINQ を使用して、正規表現によって返される <xref:System.Text.RegularExpressions.MatchCollection> の結果に対してクエリを実行したり変更したりすることができます。

このセクションで説明する手法を使えば、半構造化されたテキスト データを XML に変換することもできます。 詳細については、「[方法 :CSV ファイルから XML を生成する (C#)](how-to-generate-xml-from-csv-files.md)」を参照してください。

このセクションの例は、次の 2 つのカテゴリに分かれています。

## <a name="querying-a-block-of-text"></a>テキスト ブロックに対するクエリ

<xref:System.String.Split%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> メソッドを使用して、テキスト ブロックをクエリ可能な小さな文字列の配列に分割することによって、テキスト ブロックのクエリ、分析、および変更を実行できます。 単語や文、段落、ページなどの単位にソース テキストを分割できるほか、クエリ内で必要であれば、さらに細かく分割することもできます。

- [方法: 文字列での単語の出現回数をカウントする (LINQ) (C#)](how-to-count-occurrences-of-a-word-in-a-string-linq.md)  
  テキストに対する単純なクエリを LINQ で行う方法が紹介されています。

- [方法: 指定された単語のセットを含む文章を照会する (LINQ) (C#)](how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq.md)

  区切りを指定してテキスト ファイルを分割する方法やその各構成要素に対してクエリを実行する方法が紹介されています。

- [方法: 文字列内の文字をクエリする (LINQ) (C#)](how-to-query-for-characters-in-a-string-linq.md)

  文字列がクエリ可能型であることを実証します。

- [方法: LINQ クエリと正規表現を組み合わせる (C#)](how-to-combine-linq-queries-with-regular-expressions.md)

  LINQ クエリで正規表現を使い、フィルター処理されたクエリ結果に対して複雑なパターン マッチを行う方法が紹介されています。

## <a name="querying-semi-structured-data-in-text-format"></a>半構造化されたテキスト形式データに対するクエリ

テキスト ファイルにはさまざまな種類がありますが、タブ区切りファイルやコンマ区切りファイル、固定長行など同様の形式を持った一連の行で構成されていることは少なくありません。 そのようなテキスト ファイルをメモリに読み込んだ後、LINQ を使って、必要な行を照会したり編集したりすることができます。 複数ソースからのデータを組み合わせる作業も LINQ クエリなら簡単に行うことができます。

- [方法: 2 つのリストの差集合を見つける (LINQ) (C#)](how-to-find-the-set-difference-between-two-lists-linq.md)

  ある特定のリストには存在しているものの、それ以外には存在しない文字列をすべて探す方法が紹介されています。

- [方法: 任意の単語またはフィールドを基準にテキスト データの並べ替えまたはフィルター処理を実行する (LINQ) (C#)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)

  単語やフィールドに基づいてテキスト行を並べ替える方法が紹介されています。

- [方法: 区切り記号入りファイルのフィールドの順序を変更する (LINQ) (C#)](how-to-reorder-the-fields-of-a-delimited-file-linq.md)

  .csv ファイルの行に含まれるフィールドを並べ替える方法が紹介されています。

- [方法: 文字列コレクションを結合および比較する (LINQ) (C#)](how-to-combine-and-compare-string-collections-linq.md)

  文字列リストをさまざまな方法で結合する方法が紹介されています。

- [方法: 複数のソースからオブジェクト コレクションにデータを設定する (LINQ) (C#)](how-to-populate-object-collections-from-multiple-sources-linq.md)

  複数のテキスト ファイルをデータ ソースとしてオブジェクトのコレクションを作成する方法が紹介されています。

- [方法: 異種ファイルのコンテンツを結合する (LINQ) (C#)](how-to-join-content-from-dissimilar-files-linq.md)
  
  2 つのリストに含まれる文字列を、一致するキーを使って 1 つの文字列に結合する方法が紹介されています。

- [方法: グループを使用して 1 つのファイルを複数のファイルに分割する (LINQ) (C#)](how-to-split-a-file-into-many-files-by-using-groups-linq.md)
  
  1 つのファイルをデータ ソースとして新しいファイルを作成する方法が紹介されています。

- [方法: CSV テキスト ファイルの列値を計算する (LINQ) (C#)](how-to-compute-column-values-in-a-csv-text-file-linq.md)
  
  .csv ファイルでテキスト データに対して数学的計算を実行する方法が紹介されています。

## <a name="see-also"></a>関連項目

- [統合言語クエリ (LINQ) (C#)](index.md)
- [方法: CSV ファイルから XML を生成する](how-to-generate-xml-from-csv-files.md)
