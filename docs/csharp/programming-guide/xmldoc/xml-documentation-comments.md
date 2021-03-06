---
title: XML ドキュメント コメント - C# プログラミング ガイド
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- cs.xml
helpviewer_keywords:
- XML [C#], code comments
- comments [C#], XML
- documentation comments [C#]
- C# source code files
- C# language, XML code comments
- XML documentation comments [C#]
ms.assetid: 803b7f7b-7428-4725-b5db-9a6cff273199
ms.openlocfilehash: c40f8ee189733aa1ae58f8e46c3b7cce005ad9d7
ms.sourcegitcommit: e39d93d358974b9ed4541cedf4e25c0101015c3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2019
ms.locfileid: "55204731"
---
# <a name="xml-documentation-comments-c-programming-guide"></a>XML ドキュメント コメント (C# プログラミング ガイド)
Visual C# では、ソース コード内で、コード ブロックの直前の特別なコメント フィールド (3 個のスラッシュで示す) に XML 要素を配置することで、コードのドキュメントを作成できます。例を次に示します。  
  
```csharp  
/// <summary>  
///  This class performs an important function.  
/// </summary>  
public class MyClass {}  
```  
  
 [/doc](../../../csharp/language-reference/compiler-options/doc-compiler-option.md) オプションを使用してコンパイルすると、コンパイラは、ソース コード内のすべての XML タグを検索して、XML ドキュメント ファイルを作成します。 コンパイラによって生成されたファイルに基づいて最終的なドキュメントを作成するには、カスタム ツールを作成するか、[DocFX](https://dotnet.github.io/docfx/) や [Sandcastle](https://github.com/EWSoftware/SHFB) などのツールを使用します。  
  
 XML 要素を参照するには (たとえば、XML ドキュメント コメントに記述する特定の XML 要素を関数で処理する場合)、標準の引用のしくみを使用できます (`<` と `>`)。  コード参照 (`cref`) 要素でジェネリック識別子を参照するには、エスケープ文字 (たとえば、`cref="List&lt;T&gt;"`) または中かっこ (`cref="List{T}"`) を使用できます。  特殊なケースとして、コンパイラは中かっこを山かっことして解析し、ジェネリック識別子を参照するときにドキュメント コメントの編集があまり面倒にならないようにしています。  
  
> [!NOTE]
>  XML ドキュメント コメントはメタデータではなく、コンパイルされたアセンブリに含まれないため、リフレクションでアクセスできません。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
-   [ドキュメント コメントとして推奨されるタグ](../../../csharp/programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)  
  
-   [XML ファイルの処理](../../../csharp/programming-guide/xmldoc/processing-the-xml-file.md)  
  
-   [ドキュメント タグの区切り記号](../../../csharp/programming-guide/xmldoc/delimiters-for-documentation-tags.md)  
  
-   [方法: XML ドキュメント機能を使用する](../../../csharp/programming-guide/xmldoc/how-to-use-the-xml-documentation-features.md)  
  
## <a name="related-sections"></a>関連項目  
 詳細については次を参照してください:  
  
-   [/doc (ドキュメント コメントの処理)](../../../csharp/language-reference/compiler-options/doc-compiler-option.md)  
  
## <a name="c-language-specification"></a>C# 言語仕様  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../../../csharp/programming-guide/index.md)
