---
title: '方法: デザイナーを使用して Windows フォーム ListView コントロールに列を追加します。'
ms.date: 03/30/2017
helpviewer_keywords:
- ListView control [Windows Forms], adding column headers
- columns [Windows Forms], adding to ListView controls
ms.assetid: 5b1a8b4d-587e-479a-95c1-f9b90884f13a
ms.openlocfilehash: aaa26bad5d59f52b4fcba29fd9897aeee5cc9a0f
ms.sourcegitcommit: bef803e2025642df39f2f1e046767d89031e0304
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2019
ms.locfileid: "56303323"
---
# <a name="how-to-add-columns-to-the-windows-forms-listview-control-using-the-designer"></a>方法: デザイナーを使用して Windows フォーム ListView コントロールに列を追加します。
Windows フォーム<xref:System.Windows.Forms.ListView>コントロールが各リストの複数の列を表示できる項目の場合に、**詳細**ビュー。 いくつかの種類の各リスト項目に関する情報を表示するのに列を使用することができます。 たとえば、ファイルの一覧には、ファイル名、ファイルの種類、サイズ、およびファイルの最終更新日を表示できます。 作成されると、列の設定方法の詳細については、次を参照してください。[方法。Windows での列にサブ項目を表示フォーム ListView コントロール](../../../../docs/framework/winforms/controls/how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md)します。  
  
 次の手順が必要です、 **Windows アプリケーション**プロジェクトが含まれているフォームを<xref:System.Windows.Forms.ListView>コントロール。 このようなプロジェクトの設定の詳細については、次を参照してください。[方法。Windows フォーム アプリケーション プロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)と[方法。Windows フォームにコントロールを追加](../../../../docs/framework/winforms/controls/how-to-add-controls-to-windows-forms.md)します。  
  
> [!NOTE]
>  実際に画面に表示されるダイアログ ボックスとメニュー コマンドは、アクティブな設定またはエディションによっては、ヘルプの説明と異なる場合があります。 設定を変更するには、 **[ツール]** メニューの **[設定のインポートとエクスポート]** をクリックします。 詳細については、「[Visual Studio IDE のカスタマイズ](/visualstudio/ide/personalizing-the-visual-studio-ide)」を参照してください。  
  
### <a name="to-add-columns-in-the-designer"></a>デザイナーで列を追加するには  
  
1.  **プロパティ**ウィンドウで、設定、コントロールの<xref:System.Windows.Forms.ListView.View%2A>プロパティを<xref:System.Windows.Forms.View.Details>します。  
  
2.  **プロパティ**ウィンドウで、をクリックして、**省略記号**ボタン (![VisualStudioEllipsesButton スクリーン ショット](../../../../docs/framework/winforms/media/vbellipsesbutton.png "vbEllipsesButton")) 横<xref:System.Windows.Forms.ListView.Columns%2A>プロパティ。  
  
     **ColumnHeader コレクション エディター**が表示されます。  
  
3.  使用して、**追加**新しい列を追加するボタンをクリックします。 列ヘッダーを選択し、そのテキスト (列のキャプション)、テキストの配置、および幅を設定できます。  
  
## <a name="see-also"></a>関連項目
- [ListView コントロールの概要](../../../../docs/framework/winforms/controls/listview-control-overview-windows-forms.md)
- [Windows フォーム ListView コントロールで項目追加および削除](../../../../docs/framework/winforms/controls/how-to-add-and-remove-items-with-the-windows-forms-listview-control.md)
- [Windows フォーム ListView コントロールでの列にサブ項目を表示](../../../../docs/framework/winforms/controls/how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md)
- [Windows フォーム ListView コントロールのアイコンを表示します。](../../../../docs/framework/winforms/controls/how-to-display-icons-for-the-windows-forms-listview-control.md)
- [TreeView コントロールまたは ListView コントロール (Windows フォーム) にカスタム情報を追加します。](../../../../docs/framework/winforms/controls/add-custom-information-to-a-treeview-or-listview-control-wf.md)
