---
title: '方法: Windows フォーム DataGridView コントロールにデータをバインドします。'
ms.date: 02/08/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [Windows Forms], grids
- data binding [Windows Forms], DataGridView control
- DataGridView control [Windows Forms], data binding
ms.assetid: 1660f69c-5711-45d2-abc1-e25bc6779124
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 0d9b72766ce2e93472a07eebdf7bf59cc7b0328d
ms.sourcegitcommit: 30e2fe5cc4165aa6dde7218ec80a13def3255e98
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/13/2019
ms.locfileid: "56220575"
---
# <a name="how-to-bind-data-to-the-windows-forms-datagridview-control"></a>方法: Windows フォーム DataGridView コントロールにデータをバインドします。

<xref:System.Windows.Forms.DataGridView>コントロールは、さまざまなデータ ソースにバインドできるように、標準 Windows フォーム データ バインディング モデルをサポートしています。 バインドする、通常、<xref:System.Windows.Forms.BindingSource>データ ソースとの相互作用を管理します。 <xref:System.Windows.Forms.BindingSource>を選択するか、データの場所を変更するときに、優れた柔軟性を提供する Windows フォームのデータ ソースを指定できます。 データ ソースの詳細については、<xref:System.Windows.Forms.DataGridView>サポートを制御しを参照してください、 [DataGridView コントロールの概要](../../../../docs/framework/winforms/controls/datagridview-control-overview-windows-forms.md)します。  

Visual Studio は広範なデータ バインド DataGridView コントロールをサポートしています。 詳細については、「[方法 :データ デザイナーを使用して Windows フォームの DataGridView コントロールをバインド](https://msdn.microsoft.com/library/33w255ac\(v=vs.110\))します。  

DataGridView コントロールをデータに接続します。

1. データの取得の詳細を処理するメソッドを実装します。 次のコード例の実装を`GetData`を初期化するメソッド、 <xref:System.Data.SqlClient.SqlDataAdapter>、設定を使用して、<xref:System.Data.DataTable>します。 バインドし、<xref:System.Data.DataTable>を<xref:System.Windows.Forms.BindingSource>します。 

2. フォームの<xref:System.Windows.Forms.Form.Load>イベント ハンドラー、バインド、<xref:System.Windows.Forms.DataGridView>への制御、<xref:System.Windows.Forms.BindingSource>を呼び出すと、`GetData`データを取得します。  

## <a name="example"></a>例

この完全なコード例では、Windows フォーム DataGridView コントロールを作成するデータベースからデータを取得します。 フォームでは、データを再読み込みし、データベースへの変更を送信するボタンもあります。  

この例で必要な要素は次のとおりです。 

- SQL Server の Northwind サンプル データベースへのアクセス。 Northwind サンプル データベースのインストールの詳細については、次を参照してください。 [ADO.NET コード サンプルについては、サンプル データベースを取得](../../data/adonet/sql/linq/downloading-sample-databases.md)します。 

- System、System.Windows.Forms、System.Data、および System.Xml アセンブリへの参照。  

ビルドし、この例を実行するにコードを貼り付ける、 *Form1*新しい Windows フォーム プロジェクトでコード ファイル。  詳細については、「[方法 :Visual Studio を使用して完全な Windows フォーム コードの例をコンパイルして](https://msdn.microsoft.com/library/Bb129228\(v=vs.110\))します。 ビルドについては、C#または Visual Basic のコマンド ラインで、「 [csc.exe を](/csharp/language-reference/compiler-options/command-line-building-with-csc-exe)または[コマンドラインからビルド](/visual-basic/reference/command-line-compiler/building-from-the-command-line)します。  
  
設定、`connectionString`例では、SQL Server の Northwind サンプル データベース接続の値を持つ変数です。 Windows 認証、統合のセキュリティとも呼ばれます。 は、接続文字列にパスワードを格納するよりもデータベースに接続するより安全な方法です。 接続セキュリティの詳細については、次を参照してください。[接続情報を保護する](../../data/adonet/protecting-connection-information.md)します。  

[!code-csharp[System.Windows.Forms.DataGridViewBoundEditable](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewBoundEditable/CS/datagridviewboundeditable.cs)]
[!code-vb[System.Windows.Forms.DataGridViewBoundEditable](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewBoundEditable/VB/datagridviewboundeditable.vb)]  
  
## <a name="see-also"></a>関連項目
- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.BindingSource>
- [Windows フォーム DataGridView コントロールでデータを表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [接続情報を保護します。](../../data/adonet/protecting-connection-information.md)
